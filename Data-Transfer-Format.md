The actual Messages flowing over the wire is in this format.
```
Message{
  size int32
  data []byte
}
```
The "data" is conceptually a row of fields, with each field encoded in MsgPack format.

# Reading from Channels
When reading the final output from the flow, you can use dataset.Output() function, which returns a chan []byte object.

The []byte corresponds to the Message.data part, which is a row of MsgPack encoded fields. To decode the data, see this example:
```
package main

import (
	"fmt"

	"github.com/chrislusf/gleam/flow"
	"github.com/chrislusf/gleam/util"
)

func main() {

	luaFlow := flow.New()
	outChan := luaFlow.TextFile("/etc/passwd").FlatMap(`
		function(line)
			return line:gmatch("%w+")
		end
	`).Map(`
		function(word)
			return word, 1
		end
	`).ReduceByKey(`
		function(x, y)
			return x + y
		end
	`).Output()

	go flow.RunFlowContextSync(luaFlow)

	var word string
	var count int
	for bytes := range outChan {
		if err := util.DecodeRowTo(bytes, &word, &count); err != nil {
			fmt.Printf("decode error: %v", err)
			break
		}
		fmt.Printf("%s\t%d\n", word, count)
	}

}

```

# Input and Output format for Pipe()

Unix Pipe has a powerful toolset. But usually they operate just on a line of text. How to pass multiple variables to the next operation?

The process in Pipe() should output a row of variables separated by tabs, and separate each line by "\n".

