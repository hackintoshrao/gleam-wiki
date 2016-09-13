Reduce() operates on rows with the same key.

# How Reduce() works?

The actual code for Reduce() function is:
```
func (d *Dataset) Reduce(code string) (ret *Dataset) {
	return d.LocalSort().LocalReduce(code).MergeSortedTo(1).LocalReduce(code)
}
```

It will LocalSort() on each partition, and LocalReduce on each partition. Then MergeSortedTo() on all the sorted and locally reduced partitions, to merge into one partition, and then LocalReduce on the final partition.

# Possible Performance Improvements
If all your data does not have lots of duplicated keys in each partition, the early LocalReduce() may not help much. Theoretically you can optimize it a bit into ```d.LocalSort().MergeSortedTo(1).LocalReduce(code)```