下载git_v1.9.5

### 设置SSH通信
1.GitHub选择的默认通信方式是SSH，所以要先在Git里面生成SHH Key，打开Git Bash在其中输入如下命令：
```
ssh-keygen -t rsa -C "yourname@gmail.com"
```
之后会一路回车，就OK了。

2.在c盘，当前用户文件夹下，有个.ssh 文件夹，在里边 找到 id_rsa.pub文件，用记事本打开，复制其中的全部内容。
接着转到github站点项目编辑（edit），找到”Deploy keys“选项后点击”add another deploy key“并将刚才复制的内容黏贴保存。

3.测试SSH连接。在git bash中执行以下命令：
```
SSH -v git@github.com
```

如果提示你的密钥不正确，那么你需要重新确认上一步的操作是否完整无误。

4.如果上一步测试无错，那么现在就可以将本地的文件提交到github仓库了。在git bash中执行以下命令：

git push origin master

### 建立本地git仓库
git要求使用者必须提供自己的身份标识，为此我们需要在git bash中执行以下命令：

git config --global user.name 'yourname'

git config --global user.email youname@gmail.com

选择git仓库目录,建立项目并初始化git仓库
cd /d/mygit
git init

git将在目录下创建一个隐藏目录（.git），这个目录就是git用来管理软件版本的仓库。


### 使用git管理项目

现在我们可以开始使用git管理我们的项目：
```
cp /d/mygit
git add README 
git commit -m "这是我们第一次初始化项目"

//git add命令可以将参数指定的文件添加到git仓库索引中，如果你一次添加太多文件可以使用：git add . 命令全部添加。
//git commit命令才是真正的将文件添加到git仓库中去，-m选项允许在命令行后直接给出每次添加的简短说明.

git remote add origin git@github.com:singkuanpai/StudyNotes.git
git push -u origin master 
```
//把本地 master 分支 推送到 服务器的master分支上，如果服务器没有此分支，就 新建 此分支。这也是 在服务器上新建分支的一种方法
这个git@github.com:singkuanpai/StudyNotes.git就是上面创建项目是生成的地址。现在打开你的项目网址，你就可以发现你的代码已经展示出来了。

### git 常用命令
说明/备注	命令	备注
保存更新	git add [-i]	-i 逐个确认

检查更新	git status	

提交更新	git commit [-a] -m "<更新说明>"	-a 包含增删 -m 说明信息

克隆到本地	git clone   git@github.com:singkuanpai/StudyNotes.git

删除文件    git rm -r  git@github.com:singkuanpai/folder

远端抓取	git fetch	

与本地当前branch合并	git merge	

抓取并合并	git pull [<远端别名>] [<远端branch>] 相当于 git fetch + git merge

推送到远端	git push [-f] [<远端别名>] [<远端branch>] -f 强制覆盖

设置一个远端别名	git remote add 别名 git地址	

列出远端	git remote -v	-v 详细信息

查看远端信息	git remote show <远端别名>	

重命名远端	git remote rename <远端别名> <新远端别名>

删除远端	git remote rm <远端别名>	

更新branch列表	git remote update [<远端别名>]	

列出branch	git branch [-r] [-a]	-r 远端 -a 全部

新建branch	git branch branch名

切换branch	git checkout branch名

创建本地branch对应远端branch	git checkout -b <本地branch> -t <远端别名>/<远端branch> -b 新建branch；-t 绑定远端branch

设置HTTP代理	git config --global http.proxy "<HTTP代理>"

恢复默认	Win系统编辑 %userprofile%\.gitconfig 文件；Linux系统编辑 ~/.gitconfig 文件。

设置电子邮件	git config --global user.email "<电子邮件>"

设置用户名	git config --global user.name "<用户名>"

查看标签(tag)	git tag [--list]

打包、快照(snapshot)	git archive [--prefix=<前缀路径>/] -o <文件名及格式> branch或标签 --prefix 指定前缀路径；格式可以是 zip, tar

### Github协同流程：
fork给自己 → clone到本地 → coding → push回自己 → github上提出Pull Request即可
之后，本地添加fork源为远端源 → 工作前先pull下fork源保持代码较新 → coding → ...

