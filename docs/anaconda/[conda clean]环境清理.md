
# [conda clean]环境清理

使用`conda clean`清理缓存和不使用的包，参考[conda clean](https://docs.conda.io/projects/conda/en/latest/commands/clean.html)

## 定义

```
usage: conda clean [-h] [-a] [-i] [-p] [-t] [-f]
                   [-c TEMPFILES [TEMPFILES ...]] [-d] [--json] [-q] [-v] [-y]
```

##  示例

清理所有缓存和不使用的包

```
$ conda clean -a
```