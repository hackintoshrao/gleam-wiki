Output() returns a channel of encoded []byte back to the driver program.
SaveTextTo() is a convenient method, and actually is also an example to read from the channel.

Same as Source() functions, Output() functions are run as part of the driver program in a distributed environment. They are usually written in Go.

# Output()
Output() returns a channel of []byte. The []byte is a row of fields encoded in MsgPack format.

To decode the encoded row,
```
  import "github.com/chrislusf/gleam/util"
  ...
  decodedObjects, err = util.DecodeRow(encodedBytes)
```

# SaveTextTo(writer io.Writer, format string)

SaveTextTo() will get the channel returned from Output(), start the flow, and read out the row. The simplified code is:
```

func (d *Dataset) SaveTextTo(writer io.Writer, format string) {
	inChan := d.Output()
	var wg sync.WaitGroup
	wg.Add(1)
	go func() {
		defer wg.Done()
		for encodedBytes := range inChan {
			decodedObjects, _ := util.DecodeRow(encodedBytes)
			fmt.Fprintf(writer, format, decodedObjects...)
			writer.Write([]byte("\n"))
		}
	}()
	wg.Add(1)
	RunFlowContext(&wg, d.FlowContext)
	wg.Wait()
}
```

# SaveFinalRowTo(decodedObjects ...interface{})
If there are only one row of data in the output, the values will be set to provided objects.
```
var word string
var count int
flow.New().....SaveFinalRowTo(&word, &count)
```