
# [pkg]打包可执行文件

使用[vercel/pkg](https://github.com/vercel/pkg)将`nodeJS`工程打包成可执行文件，不再需要依赖外部的`node`环境

## 安装

```
$ npm install -g pkg
```

## 使用

打包整个工程，参考[ zjykzj/zlogo](https://github.com/zjykzj/zlogo)

```
$ pkg . -t node12-linux-x64 -o ../zlogo/tool/logo -d
```

* `.`表示当前路径，将其指向`Node`工程根目录
* `-t`表示打包的目标环境，当前指定为`Linux x64`。参考

```
nodeRange node${n} or latest
platform freebsd, linux, alpine, macos, win
arch x64, x86, armv6, armv7
```

* `-o`表示输出文件路径
* `-d`表示打印打包过程的详细信息

除了上面通过`package.json`进行打包外，还可以指定待打包的`js`文件

```
Path to entry file. Suppose it is /path/app.js, then packaged app will work the same way as node /path/app.js
Path to package.json. Pkg will follow bin property of the specified package.json and use it as entry file.
Path to directory. Pkg will look for package.json in the specified directory. See above.
```

## 配置

`pkg`会根据入口`js`文件的`require`调用进行递归搜索，但是如果使用`require(variable)`调用或者某一些非`js`文件时，需要额外指定。配置`package.json`文件，添加属性

```
  "pkg": {
    "scripts": "build/**/*.js",
    "assets": "views/**/*"
  }
```

使用`scripts`添加的文件会使用`v8::ScriptCompiler`进行编译，使用`assets`添加的文件直接保存到可执行文件中。示例如下

```
  "pkg": {
    "scripts": [
      "lib/*.js"
    ],
    "assets": [
      "fonts/*.ttf"
    ]
  },
```

## 额外

可以在`package.json`中设置`scripts`命令

```
  "scripts": {
    "build": "pkg . -t node12-linux-x64 -o ../zlogo/tool/logo -d"
  }
```

这样直接调用`npm run build` 即可完成编译