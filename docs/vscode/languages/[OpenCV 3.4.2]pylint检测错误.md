
# [OpenCV 3.4.2]pylint检测错误

利用`opencv`源码编译好`python`库后，配置在`anaconda`包中，在`vscode`上编辑`python`文件，出现如下问题：

    Module 'cv2' has no 'imread' member
    Module 'cv2' has no 'imshow' member
    Module 'cv2' has no 'waitKey' member

参考：

[【阿修】Python：解决vscode中pylint提示E1101:Module 'cv2' has no 'imshow' member](https://www.bilibili.com/video/av33693850/)

[vs code中pylint报错E1101问题的解决](https://www.jianshu.com/p/ab72e830b2a9)

修改工程配置文件`.vscode/settings.json`，添加

    "python.linting.pylintArgs": ["--generate-members"]

就可以避免错误提示了