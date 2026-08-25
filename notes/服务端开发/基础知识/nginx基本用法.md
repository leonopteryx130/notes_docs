# nginx基本用法

### nginx主进程pid号会被写入文件：

nginx会将主进程的pid号存在一个独立的文件中，这个文件在nginx安装目录下的./logs/nginx.pid文件中，这个文件的默认存储位置可以更改。示例：

<div align="left">
    <img src=./nginx基本用法1.png width=60% />
</div>
<div align="left">
    <img src=./nginx基本用法2.png width=60% />
</div>

### 设置配置文件路径：

nginx -c命令可以用来设置配置文件的路径，即nginx.conf
```
nginx -c /usr/local/nginx/conf/nginx.conf
```

### 查看当前所生效的配置文件路径：

nginx -t 可以找到当前的nginx的加载的配置文件的地址，并且可以检查nginx配置文件是否有语法错误

<div align="left">
    <img src=./nginx基本用法3.png width=100% />
</div>

### 在nginx.conf配置文件中引用其他文件：

使用include命令可以引入其他nginx的配置块，相当于把其他配置文件直接粘贴到include的位置，举个例子,
主文件：

```
…   
server {
  listen       7000;
  server_name  localhost;

  location / {
    proxy_pass http://localhost:9000/;
    proxy_set_header Host $http_host;
  }
  include include/test.conf;
}
```
include/test.conf文件中可以直接这样写:

```
location /test {
    proxy_pass http://localhost:8000/;
    proxy_set_header Host $http_host;
}
```
通过include引入即可生效

也可以用这种写法：

```
include include/*.conf;
```
即引入include目录下所有以.conf结尾的文件