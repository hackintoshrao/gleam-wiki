Pipe works exactly like Unix Pipes.

# Pipe() to Pipe()

This is the typical Unix Pipe usage. The textual lines are output from one dataset as input to the next dataset.

# Passing Tuples Pipe() to and from other APIs

Pipe usually works on text line by line, which means the input and output usually is a string.

When a tuple is passed in to Pipe(), the tuple is converted to tab-separated fields.

When Pipe() need to output a tuple, the tuple should be converted to tab-separated fields also.

# PipeArgs()

A typical use case is that we need to process a list of file names. For example, we need to use the files content
as input to the next dataset.

```
  fileNames := []string{"1.txt", "2.txt", "3.txt"}
  flow.New().Lines(fileNames).PipeArgs("cat $1").Map(...).Reduce(...)...
  gzippedFileNames = []string{"1.txt.gz", "2.txt.gz", "3.txt.gz"}
  flow.New().Lines(gzippedFileNames).PipeArgs("zcat $1").Map(...).Reduce(...)...
```


