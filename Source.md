Source() TextFile() Lines() Slice() Channel() are functions to generate initial data.

They are run as part of the driver program in a distributed environment, and can not be sent over the wire.
So they are usually written in Go.

# Listen(network, address string)
Listen() receives textual input via a socket.

# Source(f func(chan []byte))
Source() receives a function that can generate data and send to the chan[]byte.

# TextFile(fname string)
TextFile() reads the file and output text lines.

# Channel(ch chan []byte)
Channel() receives data from the channel and send out as output.

# Slice(slice [][]byte)
Slice() send out the []byte one by one as output.

# Lines(lines []string)
Lines() send out the string one by one as output.

