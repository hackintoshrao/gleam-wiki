Output() returns a channel of encoded []byte back to the driver program.
SaveTextTo() is a convenient method, and actually is also an example to read from the channel.

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