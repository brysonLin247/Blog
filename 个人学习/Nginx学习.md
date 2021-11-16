> Nginx是一款轻量级的HTTP服务器，采用事件驱动的异步非阻塞处理方式框架，这让其具有极好的IO性能，时常用于服务端的反向代理和负载均衡。

# Nginx的优点

- **支持海量高并发**：采用IO多路复用epoll。官方测试Nginx能够支持5万并发链接，实际生产环境中可以支撑 2-4万并发连接数。
- **内存消耗少**：在主流的服务器中Nginx目前是内存消耗最小的了，比如我们用Nginx+PHP，在3万并发链接下，开启10个Nginx进程消耗150M内存。
- **免费使用可以商业化**：Nginx为开源软件，采用的是2-clause BSD-like协议，可以免费使用，并且可以用于商业。
- **配置文件简单**：网络和程序配置通俗易懂，即使非专业运维也能看懂。



当然它的优点还有很多，比如 反向代理，负载均衡功能等。


# Nginx基本配置文件
## nginx.conf文件解读
nginx.conf 文件是Nginx总配置文件，在我们搭建服务器时经常调整的文件，一般在etc/nginx目录下
```bash
#运行用户，默认即是nginx，可以不进行设置
user  nginx;
#Nginx进程，一般设置为和CPU核数一样
worker_processes  1;   
#错误日志存放目录
error_log  /var/log/nginx/error.log warn;
#进程pid存放位置
pid        /var/run/nginx.pid;


events {
    worker_connections  1024; # 单个后台进程的最大并发数
}


http {
    include       /etc/nginx/mime.types;   #文件扩展名与类型映射表
    default_type  application/octet-stream;  #默认文件类型
    #设置日志模式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;   #nginx访问日志存放位置

    sendfile        on;   #开启高效传输模式
    #tcp_nopush     on;    #减少网络报文段的数量

    keepalive_timeout  65;  #保持连接的时间，也叫超时时间

    #gzip  on;  #开启gzip压缩

    include /etc/nginx/conf.d/*.conf; #包含的子配置项位置和文件
}
```
## default.conf配置项详解
进入conf.d目录查看
```bash
server {
    listen       80;   #配置监听端口
    server_name  localhost;  //配置域名

    #charset koi8-r;     
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;     #服务默认启动目录
        index  index.html index.htm;    #默认访问文件
    }

    #error_page  404              /404.html;   # 配置404页面

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;   #错误状态码的显示页面，配置后需要重启
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```
明白了这些配置项，我们知道我们的服务目录放在了 /usr/share/nginx/html 下。


# 自定义错误页和访问设置
## 多错误指向一个页面
在/etc/nginx/conf.d/default.conf中：
```bash
 error_page   500 502 503 504  /50x.html;   #错误状态码的显示页面，配置后需要重启
```
error_page指令用于自定义错误页面，/50.html用于表示当上述指定的任意一个错误的时候，都是用网站根目录下的/50.html文件进行处理。


## 单独为错误置顶处理方式
有些时候是要把这些错误页面单独的表现出来，给用户更好的体验。所以就要为每个错误码设置不同的页面。设置方法如下：
```bash
error_page  404              /404.html;   # 配置404页面
```
然后到网站目录下新建一个404.html文件，并写入一些信息。
```bash
<html>
<meta charset="UTF-8">
<body>
<h1>404页面没有找到!</h1>
</body>
</html>
```
然后重启我们的服务 【nginx -s reload】
![image.png](https://cdn.nlark.com/yuque/0/2021/png/2777137/1623212057147-fe93b621-72cc-4804-abf1-6bab1276076b.png#align=left&display=inline&height=153&margin=%5Bobject%20Object%5D&name=image.png&originHeight=153&originWidth=311&size=9552&status=done&style=none&width=311)

## 把错误码换成一个地址
处理错误的时候，不仅可以只使用本服务器的资源，还可以使用外部的资源。比如我们将配置文件设置成这样。
```bash
error_page 404 https://brysonlin247.github.io
```


## 简单实现访问控制
有时候我们的服务器只允许特定主机访问，比如内部OA系统，或者应用的管理后台系统，更或者是某些应用接口，这时候我们就需要控制一些IP访问，我们可以直接在location里进行配置。
可以直接在default.conf里进行配置。

```bash
location / {
	deny   123.9.51.42
  allow  45.76.212.231
}
```
配置完成后，重启一下服务器就可以实现限制和允许访问了。

# Nginx访问权限详讲
前面知道deny是禁止访问，allow是允许访问。但Nginx的访问控制还是比较复杂的。
## 指令优先级
```bash
location / {
	allow  45.76.202.231;
  deny   all;
}
```
上面的配置只允许45.76.202.231进行访问，其他的IP是禁止访问的。但是如果我们把deny all指令，移动到 allow45.76.202.231之前，会发现所有的IP都不允许访问了。**这说明了一个问题：就是在同一个块下的两个权限指令，先出现的设置会覆盖后出现的设置（也就是谁先触发，谁起作用）。**
**
## 复杂访问控制权限匹配
在工作中，访问权限的控制需求更加复杂，例如，对于网站下的img（图片目录）是运行所有用户访问，但对于网站下的admin目录则只允许公司内部固定IP访问。这时候仅靠deny和allow这两个指令是无法实现的。我们需要location块来完成相关的需求匹配。


上面的需求，配置代码如下：
```bash
location =/img{
	allow all;
}
location =/admin{
	deny all;
}
```
**【 = 】**号代表精确匹配，使用了 = 后是根据其后的模式进行精确匹配。这个直接关系到我们网站的安全，一定要学会。


## 使用正则表达式设置访问权限
只有精确匹配有时是完不成我们的工作任务的，比如现在我们要禁止访问所有php的页面，php的页面大多是后台的管理或者接口代码，所以为了安全我们经常要禁止所有用户访问，而只开放公司内部访问的。
```bash
location ~\.php$ {
	deny all;
}
```
这样我们再访问的时候就不能访问以php结尾的文件了。


# Nginx设置虚拟主机
虚拟主机是指在一台物理主机服务器上划分出多个磁盘空间，每个磁盘空间都是一个虚拟主机，每台虚拟主机都可以对外提供web服务，并且互不干扰。在外界看来，虚拟主机就是一台独立的服务器主机，这意味着用户能够利用虚拟主机把多个不同域名的网站部署在同一台服务器上，而不必再为一个网站单独买一台服务器，既解决了维护服务器技术的难题，同时又极大地节省了服务器硬件成本和相关的维护费用。


## 基于端口号配置虚拟主机
基于端口号来配置虚拟主机，算是Nginx中最简单的一种方式。原理就是Nginx监听多个端口，根据不同的端口号，来区分不同的网站。
我们可以直接配置在主文件里【etc/nginx/nginx.conf】文件里，也可以配置在子配置文件里【etc/nginx/conf.d/default.conf】。也可以在conf.d文件夹下新建一个文件。


这里配置在子文件里：
修改配置文件中的server选项，这时候就会有两个server。
```bash
server {
	listen 8001；
  server_name localhost;
  root /usr/share/nginx/html/html8001;
  index index.html
}
```
编在 usr/share/nginx/html/html8001/ 目录下的index.html文件并查看结果。
```bash
<h1>welcome port 8001 Dji~</h1>
```
用容器的话需要做一下docker端口映射，容器外部才能看到浏览器的信息。


## 基于IP的虚拟主机
基于IP和基于端口的配置几乎一样，只是把 server_name 选项，配置成IP就可以了。
比如：
```bash
server {
	listen 80;
  server_name: 112.74.164.244;
  root /usr/share/nginx/html/html8001;
  index index.html;
}
```
这种演示需要多个IP的支持。


# Nginx使用域名设置虚拟主机
在真实的上线环境中，一个网站是需要域名和公网IP才可以访问的。我们在实际工作中配置最多的就是设置这种虚拟主机。


先要对域名进行解析，这样域名才能正确定位到你需要到的IP上。这里新建了两个解析，分别是：

- nginx.jspang.com :这个域名映射到默认的Nginx首页位置。
- nginx2.jspang.com : 这个域名映射到原来的8001端口的位置。



## 配置以域名为划分的虚拟主机
我们修改 etc/nginx/conf.d 目录下的default.conf 文件，把原来的80端口虚拟主机改为以域名划分的虚拟主机。
```bash
server {
	listen 80;
  server_name nginx.jspang.com;
```
我们把同目录下的8001.conf文件进行修改，改成如下：
```bash
server {
	listen 80;
  server_name nginx2.jspang.com
  location / {
  	root /usr/share/nginx/html/html8001;
    index index.html index.htm;
  }
}
```
然后我们用平滑重启的方式，进行重启，这时候我们在浏览器中访问这两个网页。
其实域名设置虚拟主机也非常简单，主要操作的是配置文件的server_name项，还需要域名解析的配合


# Nginx反向代理的设置
众所周知，我们现在的web模式基本的都是CS结构，即Client端到Server端。那代理就是在Client端和Server端之间增加一个提供特定功能的服务器，这个服务器就是我们说的代理服务器。


**正向代理**：翻墙工具，它就是一个典型的正向代理工具。它会把我们不让访问的服务器的网页请求，代理到一个可以访问该网站的代理服务器上来，一般叫做proxy服务器，再转发给客户。
![](https://cdn.nlark.com/yuque/0/2021/webp/2777137/1623225854137-7179e390-ac54-4099-b928-0965dd56b41f.webp#align=left&display=inline&height=362&margin=%5Bobject%20Object%5D&originHeight=362&originWidth=769&size=0&status=done&style=none&width=769)
简单来说就是你想访问目标服务器的权限，但是没有权限。这时候代理服务器有权限访问服务器，并且你有访问代理服务器的权限，这时候你就可以通过访问代理服务器，代理服务器访问真实服务器，把内容给你呈现出来。


**反向代理**：反向代理跟正向代理相反（需要说明的是，现在基本所有的大型网站的页面都是用了反向代理），客户端发送的请求，想要访问server服务器上的内容。发送的内容被发送到代理服务器上，这个代理服务器再把请求发送到主机设置好的内部服务器上。
![](https://cdn.nlark.com/yuque/0/2021/webp/2777137/1623226383274-347a91b0-7101-4f5a-ba65-a92cd374d7e7.webp#align=left&display=inline&height=559&margin=%5Bobject%20Object%5D&originHeight=559&originWidth=741&size=0&status=done&style=none&width=741)
通过图片的对比，应该看出一些区别，这里proxy服务器代理的并不是客户端，而是服务器，即向外部客户端提供了一个统一的代理入口，客户端的请求都要先经过这个proxy服务器。具体访问哪个服务器server是由Nginx来控制的。再简单点来讲，一般代理指代理的客户端，反向代理是代理的服务器。


## 反向代理的用途和好处

- **安全性**：正向代理的客户端能够隐藏自身信息的同时访问任意网站，这个给网络安全带来了极大的威胁。因此，我们必须把服务器保护起来，使用反向代理客户端用户只能通过外来网来访问代理服务器，并且用户并不知道主机访问的真实服务器是哪一台，可以很好的提供安全保护。
- **功能性**：反向代理的主要用途是为多个服务器提供负载均衡、缓存等功能。负载均衡就是一个网站的内容被部署在若干服务器上，可以把这些机子看成一个集群，那Nginx可以将接收到的客户端请求“均匀地”分配到这个集群中所有的服务器上，从而实现服务器压力的平均分配，也叫负载均衡。



## 最简单的方向代理
现在我们要访问 http://nginx2.jspang.com 然后反向代理到 jspang.com 这个网站。我们直接到 etc/nginx/conf.d/8001.conf 进行修改。
修改后的配置文件如下：
```bash
server {
	listen 80;
  server_name nginx2.jspang.com;
  location / {
  	proxy_pass http://jspang.com;
  }
}
```
一般我们反向代理的都是一个IP，但是代理一个域名也是可以的。


## 其他反向代理指令
常用指令：

- proxy_set_header：在将客户端请求发送给后端服务器之前，更改来自客户端的请求头信息。
- proxy_connect_timeout：配置Nginx与后端代理服务器尝试建立连接的超时时间。
- proxy_read_timeout：配置Nginx与后端服务器组发出read请求后，等待响应的超时时间。
- proxy_send_timeout：配置Nginx向后端服务器组发出write请求后，等待响应的超时时间。
- proxy_redirect：用于修改后端服务器返回的响应头中的Location和Refresh。



# Nginx适配PC或移动设备
现在很多网站都是有了PC端和H5站点的，因为这样就可以根据客户设备的不同，显示出体验更好的，不同的页面了。
这样的需求有人说拿自适应就可以搞定，比如常说的bootstrap和24格布局法，这些确实是非常好的方案，但是无论是复杂性和易用性上面还是不如分开编写的好，比如我们常见的淘宝、京东....这些大型网站就都没有采用自适应，而是用分开制作的方式。


那分开制作如何通过配置Nginx来识别出应该展示哪个页面呢？


## $http_user_agent 的使用
Nginx通过内置变量 【$http_user_agent】，可以获取到请求客户端的【userAgent】，就可以知道用户目前处于移动端还是PC端，进而展示不同的页面给用户。


操作步骤如下：

1. 在/usr/share/nginx/ 目录下新建两个文件夹，分别为：pc和mobile目录
1. 在pc和mobile目录下，新建两个index.html，文件里写入内容。
1. 进入 etc/nginx/conf.d 目录下，修改8001.conf文件，改为下面的形式：
```bash
server{
        listen 8001;
        server_name localhost;
        location / {
         root /usr/share/nginx/pc;
         if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
            root /usr/share/nginx/mobile;
         }
         index index.html;
        }
}
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/2777137/1623229533753-6a1322ad-c0bd-4e65-8ff1-7da1b5202185.png#align=left&display=inline&height=200&margin=%5Bobject%20Object%5D&name=image.png&originHeight=200&originWidth=418&size=9798&status=done&style=none&width=418)![image.png](https://cdn.nlark.com/yuque/0/2021/png/2777137/1623229548628-88275083-d42f-44d4-8bbb-de0b99714053.png#align=left&display=inline&height=144&margin=%5Bobject%20Object%5D&name=image.png&originHeight=262&originWidth=778&size=18867&status=done&style=none&width=429)
# Nginx的Gzip压缩配置
Gzip是网页的一种网页压缩技术，经过gzip压缩后，页面大小可以变为原来的30%甚至更小。更小的网页会让用户浏览的体验更好，速度更快。gzip网页压缩的实现需要浏览器和服务器的支持。
![](https://cdn.nlark.com/yuque/0/2021/webp/2777137/1623229761821-10811ece-c977-4597-97de-66939c3b3b4e.webp#align=left&display=inline&height=179&margin=%5Bobject%20Object%5D&originHeight=179&originWidth=776&size=0&status=done&style=none&width=776)
从上图可以清楚的明白，gzip是需要服务器和浏览器的同时支持的。当浏览器支持支持gzip压缩时，会在请求信息中包含Accept-Encoding:gzip，这样Nginx就会向浏览器发送经过gzip后的内容，同时在相应信息头中加入Content-Encoding: gzip，声明这是gzip后的内容，告知浏览器要先解压后才能解析输出。


## gzip的配置项
Nginx提供了专门的gzip模块，并且模块中的指令非常丰富。

- gzip：该指令用于开启或关闭gzip模块。
- gzip_buffers：设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流
- gzip_comp_level：gzip压缩比，压缩级别是 1-9，1的压缩级别最低，9的压缩级别最高。压缩级别越高压缩率越大，压缩时间越长。
- gzip_disable：可以通过该指令对一些特定的User-Agent不使用压缩功能。
- gzip_min_length：设置允许压缩的页面最小字节数，页面字节数从相应信息头的Content-length中进行获取。
- gzip_http_version：识别HTTP协议版本，其值可以是1.1或1.0
- gzip_proxied：用于设置启用或禁用从代理服务器上收到相应内容gzip压缩。
- gzip_vary：用于在响应信息头中添加Vary：Accept-Encoding，使代理服务器根据请求头中的Accept-Encoding识别是否启用gzip压缩。



## gzip最简单的配置
```bash
http {
	.....
  gzip on;
  gzip_types text/plain application/javascript text/css;
  .....
}
```
gzip on 是启动gzip模块，下面的一行是用于在客户端访问网页时，对文本、JavaScript和CSS文件进行压缩输出。
配置好后，我们就可以重启Nginx服务，让我们的gzip生效了。
如果在浏览器上，可以按F12键打开开发者工具，单机当前的请求，在标签中选择Headers，查看HTTP响应头信息。你可以清楚的看见`Content-Encoding`为gzip类型。
![](https://cdn.nlark.com/yuque/0/2021/webp/2777137/1623232257624-fc14f494-5106-495e-8a59-beb06ad7c3eb.webp#align=left&display=inline&height=504&margin=%5Bobject%20Object%5D&originHeight=504&originWidth=1280&size=0&status=done&style=none&width=1280)
证明我们成功开启了gzip。


# Nginx服务启动、停止、重启
**启动Nginx服务**
默认的情况下，Nginx是不会自动启动的，需要我们手动进行启动，当然启动Nginx的方法也不是单一的。


**nginx直接启动**
在CentOS7.4版本里（低版本是不行的），是可以直接直接使用`nginx`启动服务的。
```
nginx
```
**
**使用systemctl命令启动**
还可以使用个Linux的命令进行启动，我一般都是采用这种方法进行使用。因为这种方法无论启动什么服务，都是一样的，只是换一下服务的名字（不用增加额外的记忆点）。
```
systemctl start nginx.service
```
输入命令后，没有任何提示，那我们如何知道Nginx服务已经启动了哪？可以使用Linux的组合命令，进行查询服务的运行状况。
```
ps aux | grep nginx
```
如果启动成功会出现如下图片中类似的结果。
![](https://cdn.nlark.com/yuque/0/2021/webp/2777137/1623232328574-49a4cb0b-27f8-47e5-88b7-d1ee030b7dc0.webp#align=left&display=inline&height=59&margin=%5Bobject%20Object%5D&originHeight=59&originWidth=673&size=0&status=done&style=none&width=673)
有这三条记录，说明我们Nginx被正常开启了。
**
**停止Nginx服务的四种方法**
停止Nginx 方法有很多种，可以根据需求采用不一样的方法，我们一个一个说明。

- 立即停止服务
```
nginx  -s stop
```
这种方法比较强硬，无论进程是否在工作，都直接停止进程。

- 从容停止服务
```
nginx -s quit
```
这种方法较stop相比就比较温和一些了，需要进程完成当前工作后再停止。

- killall 方法杀死进程

这种方法也是比较野蛮的，我们直接杀死进程，但是在上面使用没有效果时，我们用这种方法还是比较好的。
```
killall nginx
```

- systemctl 停止
```
systemctl stop nginx.service
```
**
**重启Nginx服务**
有时候我们需要重启Nginx服务，这时候可以使用下面的命令。
```
systemctl restart nginx.service
```
**
**重新载入配置文件**
在重新编写或者修改Nginx的配置文件后，都需要作一下重新载入，这时候可以用Nginx给的命令。
```
nginx -s reload
```
**
**查看端口号**
在默认情况下，Nginx启动后会监听80端口，从而提供HTTP访问，如果80端口已经被占用则会启动失败。我么可以使用`netstat -tlnp`命令查看端口号的占用情况。


文章参考于：[https://juejin.cn/post/6844903701459501070#heading-7](https://juejin.cn/post/6844903701459501070#heading-7)


# 操作过程中遇到的问题
## Docker如何给运行中的容器添加映射端口？


**法一：**

1. 获得容器IP

将container_name 换成实际环境中的容器名
```
docker inspect `container_name`（或者container id） | grep IPAddress
```

2. iptable转发端口

将容器的8002端口映射到docker主机的8001端口
```
iptables -t nat -A  DOCKER -p tcp --dport 8001 -j DNAT --to-destination 172.17.0.3:8002
```
**法二：**

1. 提交一个运行中的容器为镜像
```
docker commit containerid name
```

2. 运行镜像并添加端口
```
docker run -d -p 8002:80 name /bin/bash
```


## 虚拟机端口转发进行远程访问
本地只需要使用本地地址:主机端口就可以访问docker服务
![image.png](https://cdn.nlark.com/yuque/0/2021/png/2777137/1623232973478-36bfa3eb-7dbc-478f-a711-6c0c448c41c7.png#align=left&display=inline&height=457&margin=%5Bobject%20Object%5D&name=image.png&originHeight=457&originWidth=1340&size=59675&status=done&style=none&width=1340)

















