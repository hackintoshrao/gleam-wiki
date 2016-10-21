# Installation

0) Install Go

1) Install Luajit
```
  wget http://luajit.org/download/LuaJIT-2.0.4.tar.gz
  tar zxf LuaJIT-2.0.4.tar.gz
  cd LuaJIT-2.0.4
  sudo make install
```

2) Put this customized MessagePack.lua under a folder where luajit can find it.
```
  // option 1: copy a file to lua lib path
  wget https://raw.githubusercontent.com/chrislusf/gleam/master/script/MessagePack.lua
  sudo cp MessagePack.lua /usr/local/share/luajit-2.0.4/
  // option 2: install for the machine
  sudo luarocks install --server=http://luarocks.org/dev gleam-lua
  // option 3: or install it locally
  luarocks install --server=http://luarocks.org/dev gleam-lua --local
```

3) Test Installation
```
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