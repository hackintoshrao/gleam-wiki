Output() returns a channel of encoded []byte back to the driver program.
Fprintf() is a convenient method, and actually is also an example to read from the channel.

Same as Source() functions, Output() functions are run as part of the driver program in a distributed environment. They are usually written in Go.

# Output()
Output() returns a channel of []byte. The []byte is a row of fields encoded in MsgPack format.

To decode the encoded row,
```
  import "github.com/chrislusf/gleam/util"
  ...
  decodedObjects, err = util.DecodeRow(encodedBytes)
```

# Fprintf(writer io.Writer, format string)

Fprintf() will get the data returned from Output(), and printed to the writer via given format.

# SaveFirstRowTo(decodedObjects ...interface{})
SaveFirstRowTo() is a convenient method. When there are only one row of data in the output, the values will be set to provided objects.
```
var word string
var count int
flow.New().....SaveFirstRowTo(&word, &count).Run()
```