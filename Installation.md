**Table of Contents**:

- [Install Go](#install-go)
- [Install LuaJit](#install-luajit)
- [Download the project](#download-the-project)
- [Install Dependencies](#install-dependencies)

## Install Go

Follow the instructions at https://golang.org/doc/install

## Install Luajit

Download Luajit and install.

```sh
  $ wget http://luajit.org/download/LuaJIT-2.0.4.tar.gz
  $ tar zxf LuaJIT-2.0.4.tar.gz
  $ cd LuaJIT-2.0.4
  $ sudo make install
```

Put this customized MessagePack.lua under a folder where luajit can find it.
```sh
  // option 1: copy a file to lua lib path
  $ wget https://raw.githubusercontent.com/chrislusf/gleam/master/script/MessagePack.lua
  $ sudo cp MessagePack.lua /usr/local/share/luajit-2.0.4/
```
   OR 
```sh
  $ sudo luarocks install --server=http://luarocks.org/dev gleam-lua
```
   OR 
```sh
  $ luarocks install --server=http://luarocks.org/dev gleam-lua --local
```

Test LuaJit Installation
```sh
  luajit -e "require 'MessagePack'"
```
  If it is complaining something like this, just copy the MessagePack.lua to one of the paths.
```
luajit: (command line):1: module 'MessagePack' not found:
	no field package.preload['MessagePack']
	no file './MessagePack.lua'
	no file '/usr/local/share/luajit-2.0.4/MessagePack.lua'
	no file '/usr/local/share/lua/5.1/MessagePack.lua'
	no file '/usr/local/share/lua/5.1/MessagePack/init.lua'
	no file './MessagePack.so'
	no file '/usr/local/lib/lua/5.1/MessagePack.so'
	no file '/usr/local/lib/lua/5.1/loadall.so'
```

## Download the project
```sh
$ go get github.com/chrislusf/gleam
```

OR 

```sh
$ git clone https://github.com/chrislusf/gleam.git
```

## Install Dependencies
```sh
$ cd chrislusf/gleam
$ go get ./...
```



