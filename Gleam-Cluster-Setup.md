## Setup Gleam Master
Just run ```gleam master``` one any machine. To see options:

```sh
$ ./gleam master --help
usage: gleamd master [<flags>]

Start a master process

Flags:
  --help              Show context-sensitive help (also try --help-long and --help-man).
  --address=":45326"  listening address host:port
```

## Setup Gleam Agents
Just run ```gleam agent``` on any machine. You would need to point it to the master if on a different server.

```sh
$ ./gleam agent --help
usage: gleamd agent [<flags>]

Agent that can accept read, write requests, manage executors

Flags:
  --help                      Show context-sensitive help (also try --help-long and --help-man).
  --dir="/var/folders/r6/r68t56rj71x184yh01zqjk6m0000gn/T/"
                              agent folder to store computed data
  --host=""                   agent listening host address. Required in 2-way SSL mode.
  --port=45327                agent listening port
  --master="localhost:45326"  master address
  --dataCenter="defaultDataCenter"
                              data center name
  --rack="defaultRack"        rack name
  --executor.max=8            upper limit of executors
  --executor.cpu.level=1      relative computing power of single cpu core
  --memory=1024               memory limit in MB
  --clean.restart             clean up previous dataset files
  --cpuprofile=""             cpu profile output file

```

