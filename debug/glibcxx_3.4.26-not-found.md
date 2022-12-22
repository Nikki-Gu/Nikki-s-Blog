# 'GLIBCXX\_3.4.26' not found

之前在配置环境时总是遇到这个错误：

```
ImportError: /usr/lib/x86\_64-linux-gnu/libstdc++.so.6: version \`GLIBCXX\_3.4.26‘ not found
```

上次是重新配环境解决的，这次总算找到了work的解决方法，记录一下。

主要参考[这篇文章](https://blog.csdn.net/qq\_47346664/article/details/119908775)得以解决，需要注意的是存放libstdc++.so.6的路径可能不同，需要根据报错的路径来更改，我的是存放在/usr/lib/x86_64-linux-gnu/中的，而且该路径需要sudo权限才可以编辑。



