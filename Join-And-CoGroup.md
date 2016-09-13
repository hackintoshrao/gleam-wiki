Join() and CoGroup() can join rows with the same keys together.

# Join()
Suppose we are joining a left dataset and a right dataset.
```
  left:
     key1, value1, value2
     key1, value3, value4
     key2, value5, value6
  right:
     key1, value7, value8
     key2, value9, value10
  The final join results:
     key1, value1, value2, value7, value8
     key1, value3, value4, value7, value8
     key2, value5, value6, value9, value10
```

# CoGroup()
If we CoGroup() the same left and right datasets:

```
  left:
     key1, value1, value2
     key1, value3, value4
     key2, value5, value6
  right:
     key1, value7, value8
     key2, value9, value10
  The final cogroup results:
     key1, {{value1, value2}, {value3, value4} }, {{value7, value8}}
     key2, {{value5, value6}, {{value9, value10}}
```
