
# Solving environment: failed with initial frozen solve. Retrying with flexible solve.

使用`conda`安装时经常遇到以下问题：

```
$ conda install -c conda-forge opencv=4.2.0
Collecting package metadata (current_repodata.json): done
Solving environment: failed with initial frozen solve. Retrying with flexible solve.
Solving environment: / failed with repodata from current_repodata.json, will retry with next repodata source.
...
```

参考[conda安装环境报错：Solving environment: failed with initial frozen solve.](https://blog.csdn.net/weixin_41622348/article/details/100582862)

```
# 查询版本
$ conda -V
# 升级
$ conda update -n base conda
Collecting package metadata (current_repodata.json): done
Solving environment: \ 
The environment is inconsistent, please check the package plan carefully
The following packages are causing the inconsistency:

  - https://repo.anaconda.com/pkgs/main/linux-64::anaconda==2019.07=py37_0
  - https://repo.anaconda.com/pkgs/main/linux-64::numba==0.44.1=py37h962f231_0
done

## Package Plan ##

  environment location: /home/lab305/anaconda3

  added / updated specs:
    - conda


The following packages will be downloaded:
...
...
```

升级`conda`到最新版本后再更新全部应用：

```
$ conda update --all
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /home/lab305/anaconda3/envs/pytorch1.3


The following packages will be downloaded:
```

就可以继续使用`conda`安装应用：

```
$ conda install -c conda-forge opencv=4.2.0 
Collecting package metadata (current_repodata.json): done
Solving environment: failed with initial frozen solve. Retrying with flexible solve.
Solving environment: done
Collecting package metadata (repodata.json): done
Solving environment: \ debug2: client_check_window_change: changed
debug2: channel 0: request window-change confirm 0
failed with initial frozen solve. Retrying with flexible solve.
Solving environment: done

## Package Plan ##

  environment location: /home/lab305/anaconda3/envs/pytorch1.3

  added / updated specs:
    - opencv=4.2.0


The following packages will be downloaded:
...
...
```