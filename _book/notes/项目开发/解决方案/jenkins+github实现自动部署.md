# jenkins+github实现自动部署

### 第一步：登录自己的github并生成一个person access token

点击右侧头像，左侧弹出一个抽屉，点击settings

<div align="left">
    <img src=./jenkins自动部署1.png width=40% />
</div>

然后点击底部Developer settings

<div align="left">
    <img src=./jenkins自动部署2.png width=40% />
</div>

选择Tokens(classic)

<div align="left">
    <img src=./jenkins自动部署3.png width=40% />
</div>

点击Generate new token

<div align="left">
    <img src=./jenkins自动部署4.png width=90% />
</div>

勾选repo和admin:repo_hook，Note就是名称，可以自定义

<div align="left">
    <img src=./jenkins自动部署5.png width=90% />
</div>

最后点击底部generate token按钮，会生成一个token，这个token很重要，并且只展示一次，注意保存

<div align="left">
    <img src=./jenkins自动部署6.png width=90% />
</div>

***

### 第二步：在jenkins里边配置github

在dashboard里点击Manage Jenkins

<div align="left">
    <img src=./jenkins自动部署7.png width=40% />
</div>

选择system

<div align="left">
    <img src=./jenkins自动部署8.png width=40% />
</div>

下滑找到Github，配置如下

<div align="left">
    <img src=./jenkins自动部署9.png width=70% />
</div>

Name自定义，API URL固定填这个就行，Credentials就是github上生成的person access token，tips:右侧有个test connection可以用于测试Credentials是否可用。

***

### 第三步：给自己的代码仓库创建webhook

进入到自己的项目代码里，点击代码项目的settings

<div align="left">
    <img src=./jenkins自动部署10.png width=90% />
</div>

点击webhooks

<div align="left">
    <img src=./jenkins自动部署11.png width=40% />
</div>

点击右上角Add webhook

<div align="left">
    <img src=./jenkins自动部署12.png width=90% />
</div>

配置如下：

<div align="left">
    <img src=./jenkins自动部署13.png width=90% />
</div>

其中ip和端口就是jenkins服务的ip和端口

***

### 第四步：jenkins里新建一个项目，并配置仓库和自动部署

首先点击New item新建一个项目，并进入项目中点击configure

<div align="left">
    <img src=./jenkins自动部署14.png width=40% />
</div>

配置Github project url为代码仓库url地址

<div align="left">
    <img src=./jenkins自动部署15.png width=60% />
</div>

配置Respository URL为.git的地址

<div align="left">
    <img src=./jenkins自动部署16.png width=90% />
</div>

Branches表示分支，如果为空就是全部分支，Repository browser选择githubweb，url就是仓库url地址

<div align="left">
    <img src=./jenkins自动部署17.png width=70% />
</div>

底部的Build Triggers和environment配置如下

<div align="left">
    <img src=./jenkins自动部署18.png width=100% />
</div>

至此，基本的自动部署已经实现，可以在jenkins里点击Build Now并查看console进行测试。

### 其余配置

可以在Bulid steps选择execute shell里写命令操作yarn install等命令实现前端部署，可以搭配nginx实现自动化发布