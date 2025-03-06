# github切换ssh连接

### 第一步：生成新的ssh key

以mac为例，打开终端，输入以下命令：

```bash
ssh-keygen -t rsa -C "your_email@example.com"
```

然后根据提示一直会车就行，会在命令行目录下生成一个 .ssh 文件夹，里边有 id_rsa 和 id_rsa.pub 两个文件，id_rsa 是私钥，id_rsa.pub 是公钥。

查看并复制公钥内容：

```bash
cat ~/.ssh/id_rsa.pub
```
然后复制整个公钥内容。

### 第二步：添加新的ssh key到github

登录github，点击右上角头像，选择Settings，然后选择SSH and GPG keys，点击New SSH key。
<div align="center">
    <img src=./github切换ssh连接1.jpg width=90% />
</div>
将刚才复制的公钥内容粘贴到Key里，Title可以随便写，然后点击Add SSH key。
<div align="center">
    <img src=./github切换ssh连接2.jpg width=90% />
</div>


### 第三步：修改本地git的远程仓库地址

在本地git项目文件里打开终端，输入以下命令：

```bash
git remote -v
```

查看远程仓库地址，如果地址是https开头的，则执行以下命令：

```bash
git remote set-url origin Github仓库中的ssh地址
```
Github仓库的SSH地址见
<div align="center">
    <img src=./github切换ssh连接3.jpg width=70% />
</div>

然后再次执行：

```bash
git remote -v
```

查看远程仓库地址，如果地址已经变成ssh开头的，则说明已经切换成功。
