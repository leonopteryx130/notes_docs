# npm发展

### 嵌套结构阶段

在 ```npm@2``` 的早期版本中，对应 ```Node.js 4.x``` 及以前的版本，```node_modules``` 在安装时是嵌套结构。一个简单的例子，```demo-foo``` 和 ```demo-baz``` 中均依赖 ```demo-bar```，在同时安装 ```demo-foo``` 和 ```demo-baz``` 时会生成如下的 ```node_modules``` 结构：

```
node_modules
└─ demo-foo
   ├─ index.js
   ├─ package.json
   └─ node_modules
      └─ demo-bar
         ├─ index.js
         └─ package.json
└─ demo-baz
   ├─ index.js
   ├─ package.json
   └─ node_modules
      └─ demo-bar
         ├─ index.js
         └─ package.json

```

这个时候的目录结构虽然比较清晰，但是每个依赖包都会有自己的 node_modules，相同的依赖并没有复用，例如上面的相同依赖 demo-bar 就被安装了两次

### 扁平结构阶段

为了解决上述问题，```yarn``` 提出了扁平结构的设计，将所有的依赖在 ```node_modules``` 中平铺，后来的 ```npm v3```版本的实现也与之类似，因此使用 ```yarn``` 或者 ```npm@3+``` 安装上述的例子，将会得到如下扁平式的目录结构：

```
node_modules
└─ demo-bar
   ├─ index.js
   └─ package.json
└─ demo-baz
   ├─ index.js
   └─ package.json
└─ demo-foo
   ├─ index.js
   └─ package.json
```

另外这种方式对于相同依赖的不同版本，则只会将其中一个进行提升，剩余的版本则还是嵌套在对应的包中，例如我们上面的 ```demo-foo``` 中对于 ```demo-bar``` 的依赖升级到 ```v1.0.1``` 版本，则会得到下面的结构，具体哪个版本会提升到最顶层则取决于安装时的顺序，示例：

```
node_modules
└─ demo-bar
   ├─ index.js
   └─ package.json
└─ demo-baz
   ├─ index.js
   ├─ package.json
   └─ node_modules
      └─ demo-bar
         ├─ index.js
         └─ package.json
└─ demo-foo
   ├─ index.js
   ├─ package.json
   └─ node_modules
      └─ demo-bar
         ├─ index.js
         └─ package.json
```

但是扁平结构也并不完美，比如会出现[幽灵依赖](./幽灵依赖.md)的问题

### pnpm的出现

pnpm利用全局store，软连接和硬链接的方式，解决了上述问题，具体可以参考 [pnpm的原理](。。。)。

但是其自身也有局限性，例如：由于 ```symbolic link``` 在一些场景下有兼容性问题，目前 ```Eletron``` 以及 ```labmda``` 部署的应用上无法使用 ```pnpm```。

