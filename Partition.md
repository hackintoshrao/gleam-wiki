Partition will shard the content by the key, or by the value if there are no key.

# Partition(partitionCount)
Each row's key, or the row itself if there are no key, is hashed into a number, and mod by the partitionCount, and assigned to the right partition.

Each partition's following Map/Filter/LocalSort/LocalReduce will be executed in parallel.

# Partition(partitionCount int, indexes ...int)
Same function as above. This can selectively choose to partition on which field, or fields. The fields will be moved to the front.

```
  before:
     key1, value1, key2, value2
  operation:
     Partition(1,3)
  after:
     key1, key2, value1, value2
```


# Effect
Rows with the same key or keys will be partitioned into the same partition. This makes operations on rows with the same keys easy.

# Complexity
This could be a heavy operation when you already have a lots of partitions and wants to partition again.

