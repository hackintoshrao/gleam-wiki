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

# How ReduceByKey() works?

The pseudo code code for ReduceByKey() function is:
```
func (d *Dataset) ReduceByKey(code string) (ret *Dataset) {
	return d.LocalSort().LocalReduceByKey(code).MergeSortedTo(1).LocalReduceByKey(code)
}
```

It will LocalSort() on each partition, and LocalReduceByKey() on each partition. Then MergeSortedTo() on all the sorted and locally reduced partitions, to merge into one partition, and then LocalReduceByKey() on the final partition.


# Possible Performance Improvements
If all your data does not have lots of duplicated keys in each partition, the early LocalReduceByKey() may not help much. Theoretically you can optimize it a bit into ```d.LocalSort().MergeSortedTo(1).LocalReduceByKey(code)```

Do not always use the Reduce() or ReduceByKey() directly.
