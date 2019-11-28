# 发生了什么

macOS 系统升级到 Canalina v10.15 之后，我在执行 `npm i` 的时候出现了一下的错误：

```
make: *** [Release/obj.target/hiredis/src/hiredis.o] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:194:23)
gyp ERR! stack     at ChildProcess.emit (events.js:210:5)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:272:12)
gyp ERR! System Darwin 19.0.0
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Users/youngbye/pro/gitlab/admin.sl.xunlei.com/node_modules/hiredis
gyp ERR! node -v v12.13.1
gyp ERR! node-gyp -v v5.0.5
gyp ERR! not ok
npm WARN admin.sl.xunlei.com@1.4.3 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: hiredis@0.5.0 (node_modules/hiredis):
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: hiredis@0.5.0 install: `node-gyp rebuild`
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: Exit status 1

```

## 在 Canalina 中下载node-gyp

node-gyp 的 repo 地址 [https://github.com/nodejs/node-gyp](https://github.com/nodejs/node-gyp)，阅读后其实会发现对 Canalina(v10.15) 之后的系统版本有一个md的地址 [https://github.com/nodejs/node-gyp/blob/master/macOS_Catalina.md](https://github.com/nodejs/node-gyp/blob/master/macOS_Catalina.md) 里头有完整的处理方案就不赘述了

## node-gyp 是什么？

这里有一份关于 node-gyp 的讲解，[刨根问底之node-gyp](https://github.com/tsy77/blog/issues/5)。

```
node-gyp是一个跨平台的命令行工具，目的是编译node addon模块。

常用的命令有configure和build，configure 原理就是利用gyp生成不同的编译配置文件，build则根据不同平台、不同构建配置进行编译。
```

简单的说noe-gyp是编辑C++扩展用的，node addon中存在有很多不同语言编写的工具，所以需要预先编译一下，否则就会有跨平台的问题。

