Sort will sort rows by the key, or the row value if there are no key.

# How sorting is done?

The actual code for Sort() is:
```
func (d *Dataset) Sort() *Dataset {
	return d.LocalSort().MergeSortedTo(1)
}
```

It will try to sort on each partition locally, and then merge the sorted list into one final partition.

# Caveat

If you use Pipe(), you can also use Pipe("sort"). However, this only works on one single partition. 
So if there are multiple partitions, you still need to merge the sorted partitions. It can be done by calling MergeSortedTo(1).
