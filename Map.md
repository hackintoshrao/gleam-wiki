Map() ForEach() FlatMap() Filter() all operate on a single input row.

# Map()
Map() is the basic one row to one row conversion.

The Lua allows returning multiple return results, which corresponds to a row of fields. The first row field is the key. So this Map() sometimes can change a row's key this way.
```
  Map(`
    function(key, val1, val2)
      return val1, key, val2
    end
  `)
```

# ForEach()
ForEach() can used to convert from one row into multiple rows.

writeRow(a,b,c) is a convenient function to output a row of fields.

```
  ForEach(`
      function(fileName)
        -- Open a file for read
        local fh,err = io.open(fileName)
        if err then return end

        -- line by line
        while true do
          local line = fh:read()
          if not line then break end
          writeRow(line)
        end

        -- Following are good form
        fh:close()
      end
  `)
```

# FlatMap()
FlatMap() acts like Map(), but the return result usually is an array of objects. Or a table in Lua's term.

The table of objects will be flattened and the objects will be passed one by one as input to the next step.

```
    FlatMap(`
      function(line)
        return line:gmatch("%w+")
      end
    `)
```

# Filter()
Filter() takes a row of fields, and return a boolean. If returning true, the row is passed to the next step.


```
    Filter(`
      function(word, count)
        return count > 5
      end
    `)
```

# Select(indexes ...int)
Select() takes a list of indexes, which is 1-based, to project the result.


```
  before:
     value1, value2, value3, value4
  operation:
     Select(3,1)
  after:
     value3, value1
```
