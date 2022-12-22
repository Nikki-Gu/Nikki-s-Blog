# git push error: Time out

根本原因是在国内，如果没有使用代理连接github本身就很慢，容易time out；如果使用了代理但是没有在git配置里面添加代理，也会time out。所以解决方法有两种，一种是配置代理，一种是取消代理。

PS：其实原因我好像也没有很懂，目前的理解是上面这样子，计网学得太差了ORZ

#### 配置代理

<port>替换为使用的代理对应的socks5端口和http端口，也可以都配置为混合端口。

```
git config --global http.https://github.com.proxy socks5://127.0.0.1:<port>
git config --global http.https://github.com.proxy 'http://127.0.0.1:<port>'
```

#### 取消代理

```
git config --global --unset http.https://github.com.proxy
git config --global --unset http.proxy 
git config --global --unset https.proxy
```



### git config

顺带补充一点git config的知识。

git config是对git的配置文件进行配置的命令，git的配置文件有三种：local、global、system，对应的位置和解释如下：

- local
  - 位置：.git/config 
  - 定义：仓库级配置，这个配置中的设置只对当前所在仓库有效
  - 命令：`git/config --local`，此时读写的就是这个文件
- global
  - 位置：~/.gitconfig
  - 定义：全局级配置，用户目录下的配置文件，只适用于该用户
  - 命令： `git config --global` 
- system
  - 位置：/etc/gitconfig
  - 定义：系统级配置，系统中对所有用户都适用的配置
  - 命令：`git config --system` 

每个仓库具体生效的配置是按 system、global、local 的顺序查找的，后面的同名配置会覆盖上一级的配置。

使用`git config -l` 可以查看当前生效的配置

