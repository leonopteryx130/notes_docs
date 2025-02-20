# jenkins修改shell脚本的执行用户

jenkins的shell里，默认情况下，是以jenkins用户执行的，如果需要以其他用户执行，需要修改jenkins的配置文件

### ubuntu系统下把jenkins的shell脚本执行用户修改为ubuntu

修改文件`vim /etc/default/jenkins`

将原来的
```
# Jenkins runs as root by default.
JENKINS_USER=$NAME
JENKINS_GROUP=$NAME
```
修改为
```
# Jenkins runs as root by default.
JENKINS_USER=ubuntu
JENKINS_GROUP=ubuntu
```

然后重启jenkins
```
sudo service jenkins restart
```