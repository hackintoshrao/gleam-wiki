Partition will shard the content by the key, or by the value if there are no key.

# Partition(partitionCount)
Each row's key, or the row itself if there are no key, is hashed into a number, and mod by the partitionCount, and assigned to the right partition.

Each partition's following Map/Filter/LocalSort/LocalReduce will be executed in parallel.

# Effect
Rows with the same key will be partitioned into the same group. This makes operations on rows with the same key easy.

# Complexity
This could be a heavy operation when you already have a lots of partitions and wants to partition again.


