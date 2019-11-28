## 在 Catalina 中下载node-gyp

node-gyp 的 repo 地址 [https://github.com/nodejs/node-gyp](https://github.com/nodejs/node-gyp)，阅读后其实会发现对 Catalina(v10.15) 之后的系统版本有一个md的地址 [https://github.com/nodejs/node-gyp/blob/master/macOS_Catalina.md](https://github.com/nodejs/node-gyp/blob/master/macOS_Catalina.md) 里头有完整的处理方案就不赘述了

## node-gyp 是什么？

这里有一份关于 node-gyp 的讲解，[刨根问底之node-gyp](https://github.com/tsy77/blog/issues/5)。

```
node-gyp是一个跨平台的命令行工具，目的是编译node addon模块。

常用的命令有configure和build，configure 原理就是利用gyp生成不同的编译配置文件，build则根据不同平台、不同构建配置进行编译。
```

简单的说noe-gyp是编辑C++扩展用的，node addon中存在有很多不同语言编写的工具，所以需要预先编译一下，否则就会有跨平台的问题。

