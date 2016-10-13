Reduce() operates on rows that has only one field.
ReduceByKey() operates on rows with the same key.

# How Reduce() works?

The pseudo code code for Reduce() function is:
```
func (d *Dataset) Reduce(code string) (ret *Dataset) {
	return d.LocalReduce(code).MergeSortedTo(1).LocalReduce(code)
}
```

It will LocalReduce() on each partition. Then MergeSortedTo() on all the locally reduced partitions, to merge into one partition, and then LocalReduce() on the final partition.

# How ReduceBy() works?

The pseudo code code for ReduceBy() function is:
```
func (d *Dataset) ReduceBy(code string) (ret *Dataset) {
	return d.LocalSort().LocalReduceBy(code).MergeSortedTo(1).LocalReduceBy(code)
}
```

It will LocalSort() on each partition, and LocalReduceBy() on each partition. Then MergeSortedTo() on all the sorted and locally reduced partitions, to merge into one partition, and then LocalReduceBy() on the final partition.


# ReduceBy(code string, indexes ...int)
Same function as above. This can selectively choose to reduce on which field, or fields. The fields will be moved to the front.

```
  before:
     key1, value1, key2, value2
  operation:
     ReduceBy(code, 1,3)
  after:
     key1, key2, value1, value2
```
The code is expected to be function(value1, value2).