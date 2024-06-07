#### ssh拉取代码出错
问题:
> Connection closed by 28.0.0.69 port 22 fatal: Could not read from remote repository.  Please make sure you have the correct access rights

"思考了一下近期的操作，使用新的梯子，设置了TUN模式代理了全局， 有可能造成了端口变化， 导致了git基于SSH拉取出了问题。"
**解决方法:**
1. 可以放弃SSH方式， 换用HTTPS方式来读取和拉取代码。
2. 坚持使用SSH方式，在 {用户}/.ssh/ 目录下建立config文本文档，输入以下代码，端口指向443即可。
```bash
	Host githup.com
	Hostname ssh.githup.com
	Port 443
```

**报错信息：**

> hint: Updates were rejected because the remote contains work that you do
> hint: not have locally. This is usually caused by another repository pushing
> hint: to the same ref. You may want to first integrate the remote changes
> hint: (e.g., 'git pull ...') before pushing again.
> hint: See the 'Note about fast-forwards' in 'git push --help' for details.

**问题原因：**在不同机器上做了提交

**解决方法：**

> 是不是在不同的机器上上做了提交？？
>
> 远程分支上存在本地分支中不存在的提交，往往是多人协作开发过程中遇到的问题，可以先`fetch`再`merge`，也就是`pull`，把远程分支上的提交合并到本地分支之后再`push`。
>
> 如果你确定远程分支上那些提交都不需要了，那么直接`git push origin master -f`，强行让本地分支覆盖远程分支。。。

#### 不支持账号密码访问

报错信息：

> remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
> remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
> fatal: unable to access 'https://github.com/<USERNAME>/<REPO>.git': The requested URL returned error: 403

**生成token**

1. 点击Settings.
2. 点击左侧的Developer settings
3. 点击Personal access tokens(个人访问令牌)
4. 点击Generate new token
5. 设置token信息

**修改现有项目的url**

```shell
git remote set-url origin  https://<your_token>@github.com/<USERNAME>/<REPO>.git
```

**summer:**这个问题困了我接近十个小时！！！啊！我一直以为是挂了梯子的问题，一直在这方面琢磨，没曾想居然是这个问题，这个问题之前就出现解决过，不过因为报错信息不同，我就没到到这点，出现了问题一定要写记录，方便以后复盘，我就知道，我一定是哪方面出问题了，果然！念念不忘，必有回响！

#### 解决中文显示问题

```bash
# --注释：该命令表示提交命令的时候使用utf-8编码集
git config --global i18n.commitencoding utf-8

# --注释：该命令表示日志输出时使用utf-8编码集显示
git config --global i18n.logoutputencoding utf-8

// --注释设置LESS字符集为utf-8
export LESSCHARSET=utf-8

git config --global core.quotepath false
```

