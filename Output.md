Output() returns a channel of encoded []byte back to the driver program.
Fprintf() is a convenient method, and actually is also an example to read from the channel.

Same as Source() functions, Output() functions are run as part of the driver program in a distributed environment. They are usually written in Go.

# Fprintf(writer io.Writer, format string)

Fprintf() will get the data returned from Output(), and printed to the writer via given format.

# SaveFirstRowTo(decodedObjects ...interface{})
SaveFirstRowTo() is a convenient method. When there are only one row of data in the output, the values will be set to provided objects.
```
var word string
var count int
flow.New().....SaveFirstRowTo(&word, &count).Run()
```

# Output()
Output(f func(io.Reader) error) takes a function to process a io.Reader. The []byte can be a row of fields encoded in MsgPack format, or tab-separated lines from a Pipe() or PipeAsArgs() functions.

This is an expert-level function. Follow examples in Fprintf() to learn to decode objects.
