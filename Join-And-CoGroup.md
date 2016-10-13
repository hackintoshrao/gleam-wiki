Join() and CoGroup() can join rows with the same keys together.

# Join(other *Dataset, indexes ...int)
Suppose we are joining a left dataset and a right dataset.
```
  left:
     key1, value1, value2
     key1, value3, value4
     key2, value5, value6
  right:
     key1, value7, value8
     key2, value9, value10
  operation: left.Join(right)
  The final join results:
     key1, value1, value2, value7, value8
     key1, value3, value4, value7, value8
     key2, value5, value6, value9, value10
```

# CoGroup(other *Dataset, indexes ...int)
If we CoGroup() the same left and right datasets:

```
  left:
     key1, value1, value2
     key1, value3, value4
     key2, value5, value6
  right:
     key1, value7, value8
     key2, value9, value10
  operation: left.CoGroup(right)
  The final cogroup results:
     key1, {{value1, value2}, {value3, value4} }, {{value7, value8}}
     key2, {{value5, value6}, {{value9, value10}}
```

# GroupBy(indexes ...int)
This can selectively choose to group on which field, or fields. The fields will be moved to the front.

```
  before:
     key1, value1, key2, value2
     key1, value3, key2, value4
     key3, value5, key2, value6
  operation:
     GroupBy(1,3)
  after:
     key1, key2, [[value1, value2],[value3, value4]]
     key3, key2, [[value5, value6]]
```
Specally, if only one field is a non-key field, the non-key field will be flattened by one level.

```
  before:
     key1, value1, key2
     key1, value3, key2
     key3, value5, key2
  operation:
     GroupBy(1,3)
  after:
     key1, key2, [value1, value3]
     key3, key2, [value5]
```
