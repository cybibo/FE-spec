#Git同一项目使用多个远程仓库  
某些场合，一个git项目需要能同时使用两个甚至多个远程仓库，比如公司提供了一个gitlab仓库，新智那边也是一个gitlab仓库或者我们后期会分测试环境和生产环境等。在项目的根目录查看git配置文件，一般来说是这样的：

```
$ cat .git/config 
[core]
 repositoryformatversion = 0
 filemode = false
 bare = false
 logallrefupdates = true
 symlinks = false
 ignorecase = true
 hideDotFiles = dotGitOnly
 [remote "origin"] 
url = git@10.168.4.20:daoshuFE/FE-xxxxx.git 
fetch = +refs/heads/*:refs/remotes/origin/*
 [branch "master"]
 remote = origin
 merge = refs/heads/master
```

为了方便我们操作，我们可以修改git的config配置，步骤如下：  
1.  添加一个远程仓库  
修改config文件，加入另一个远程仓库，并为其命名，比如称为mirror：
```
[remote "origin"]
 url = git@10.168.4.20:daoshuFE/FE-xxxxx.git 
 fetch = +refs/heads/*:refs/remotes/origin/*
 [remote "mirror"]
 url = git@10.39.39.18:daoshuFE/FE-xxxxx.git 
 fetch = +refs/heads/*:refs/remotes/origin/*
 [branch "master"]
 remote = origin
 remote = mirror
 merge = refs/heads/master
```

操作之前切记：新智那边的gitlab库我们要打开vpn并且连接成功后才能操作  

2. pull操作  
使用以下命令，可以分别从两个远程仓库pull：  
```
git pull origin master  
git pull mirror master  
```

3. push操作  
使用以下命令，可以分别push到两个远程仓库：  
```
git push origin master
git push mirror master
```