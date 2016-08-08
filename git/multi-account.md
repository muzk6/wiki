# 多账号设置
> [多个github帐号的SSH key切换](http://blog.csdn.net/itmyhome1990/article/details/42643233)

1. `cd`到`~/.ssh`目录下，用命令生成`ssh key`时，自定义`id_ras`名字，
一个邮箱账号对应一个，例如建两个分别为`id_rsa_github`和`id_rsa_gitlab`

2. 在`.ssh`目录下创建配置文件，名为`config`，其中`Host`和`HostName`的值最好一致，
因为仓库地址格式中的`git@Host`用的就是`Host`的值，而默认的`id_rsa`用的`Host`和`HostName`
就是一致的
```
Host github.com
    HostName github.com
    IdentityFile ~/.ssh/id_rsa_github

Host gitlab.net
    HostName gitlab.net
    IdentityFile ~/.ssh/id_rsa_gitlab
```

3. 测试连接`ssh -T git@Host`，其中`Host`就是配置文件`config`里的`Host`值

* 连接失败时，可用`ssh -v git@Host`，比如这里为`ssh -v git@github.com`查看具体调试信息
* 之前有遇过连接失败，原因就是改了`hosts`文件，使`github.com`重定向另外一个地址了，所以就连接失败

4. 如果在第2步中没把`Host`和`HostName`值设置成一致，就需要进行这一步
```
$ git remote -v
origin  git@github.com:muzk6/wiki.git (fetch)
origin  git@github.com:muzk6/wiki.git (push)
git remote set-url origin git@Host:muzk6/wiki.git
```
其中最后一行命令的`Host`为配置文件对应`id_rsa`的`Host`值