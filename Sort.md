Sort will sort rows by the key, or the row value if there are no key.

# How sorting is done?

The pseudo code for Sort() is:
```
func (d *Dataset) Sort() *Dataset {
	return d.LocalSort().MergeSortedTo(1)
}
```

It will try to sort on each partition locally, and then merge the sorted list into one final partition.

# Sort(indexes ...int)
Same function as above. This can selectively choose to sort on which field, or fields. The fields ordering will stay the same.

# Caveat

If you use Pipe(), you can also use Pipe("sort"). However, this only works on one single partition. 
So if there are multiple partitions, you still need to merge the sorted partitions. It can be done by calling MergeSortedTo(1).

