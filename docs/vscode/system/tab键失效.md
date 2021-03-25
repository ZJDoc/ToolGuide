
# `TAB`键失效

参考：[Vscode中tab键不起作用，解决方法](https://my.oschina.net/korabear/blog/1790438)

**问题一：`tab`键失效**

打开系统设置，查看`editor.tabCompletion`、`emmet.triggerExpansionOnTab`是否开启

    "editor.tabCompletion": "on",
    "emmet.triggerExpansionOnTab": true

**问题二：`tab`键缩进空格**

有两种方法

1. 点击右下角状态栏的`Spaces`选项
2. 打开系统设置，编辑属性`editor.tabSize`