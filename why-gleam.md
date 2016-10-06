# Why
Previously I created https://github.com/chrislusf/glow, a Go based distributed computing system. One thing I like is that it's all pure Go and statically compiled. However, the related problem is that it is a fixed computation flow. There are no easy way to dynamically send a different computation to remote executors.

## Dynamic Computation
Gleam resolved this issue. It can send Lua code, or shell scripts, over the wire. The computation can be adjusted dynamically.

## Efficient Flexible Luajit Execution
Gleam support Luajit in first class. Luajit has performance comparable to C, Java, or Go. The syntax is simple. Even simpler than Go. Lua was created as an embeddable language, and fits nicely with Go. Admittedly it is a scripting language and lacks type safety. But Go's type system can overlay on top of Lua, hiding the details inside reusable Go functions, providing the comfort and joy of refactoring code.

## Distributedly Parallel Unix Pipe Tools
Gleam also support Unix Pipe tool set, such as "grep", "awk", "tr", "sort", "uniq", etc. These are simple, small and super efficient tools that can be combined together to do great things. However, there are no systems that can distributedly run these tools.

Go's concurrent programming support easily enables parallel execution for the schell scripts.

## Pipe Execution for any language
Gleam's Pipe() function is basically the same as Unix's pipeline. In additional to all basic unix tools, you can use anything written in Python/Ruby/Shell/Java/C/C++, or mix them together, to do distributed computing.

# Compared to Spark
Spark is a popular system.

* No more JVM memory tuning. JVM tuning is a big headache. Enough said...
* No more systems setup. Spark needs a whole set of hadoop eco system.
* Fast to setup and run. Gleam agents and Gleam master are very simple and very fast to setup.
* Dynamically switch between Local mode, distributed in memory piping mode, or distributed disk-based RDD mode.
* Back Pressure
