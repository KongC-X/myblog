# Node.js-Web-Server-

## Node.js 从零开发 Web Server 博客项目笔记

#### 代码运行流程

首先开启服务器，在 npm run dev 的时候运行了 bin 目录下的 www.js 文件，启动 http 服务

当前端进行访问的时候，经过 app.js 文件

App.js 是整个项目的入口文件，首先判断这个用户在 http 的 header 头中带了那些验证的信息，例如带了 userid(cookie)，有 userid 就赋值给 req.session 和存入 redis,没有就创建一个

验证完信息后，进入每个模块的处理区间，例如博客、用户

例如进入到了博客区间，通过 router 文件 blog.js，通过 req.query 获取到路由，进入到当前的路由区间

通过 req.body 或 req.query 获取接口的参数，调用控制器的方法，根据参数处理数据

通过封装的处理数据模块返回固定的数据格式（succesfulModel、errorModel）

App.js 中调用 handleBlogRouter 获取到处理后的数据，转换为字符串，然后返回给前端

#### Cookie

什么是 cookie，cookie 是存在浏览器的一段字符串，格式 k1=v1;k2=v2;可存储结构化数据

Cookie 的大小，5kb

Cookie 不允许跨域，跨域不共享

每次发送 http 请求，会将请求域的 cookie 一起发送给 server

Server 端可以修改 cookie，并返回给浏览器

浏览器也可以修改 cookie，通过 document.cookie，可以限制浏览器对 cookie 的写入，写入的不能覆盖受限制的 cookie

Cookie 中不要暴露用户的敏感信息

#### Session

Session 中存储用户的信息，通过 cookie 中获取到的信息，查询出需要验证的信息

Session 存放在数据库中，如果存储在进程的内存中，会导致进程卡死，缓慢

正式上线会启用多进程，多进程中的内存无法共享

系统会限制进程的内存

#### Redis

Web server 最常用的缓存数据库，数据存放在内存中

相比 mysql 读取速度快（内存读取的速度比硬盘快的多）

成本高，可存储的数据量更少

#### Nginx

Nginx 反向代理，安装 nginx，配置 nginx.conf 文件

#### 记录日志

日志有可能比较大，存储在 redis 或 mysql 中都不太合适，存在 redis 中，如果文件比较大，相对会消耗内存，导致程序卡顿或缓慢，存储在 mysql 中，读取和查看不方便，不如文件直接，迁移到其他服务器，又要搭建 mysql 环境等

存在在文件中，解决文件过大的问题，通过管道的方式进行传输，一点一点的流到需要接收的端

做定时任务，使用 linux 的 crontab，**\***command, 分 时 天 月 星期 命令

日志逐行读取，使用 readline

#### Server 安全

预防 sql 攻击，例如注释掉部分条件语句，通过传进来的 sql

Xss 攻击，往服务器中添加 js 代码，预防主要是过滤掉<>尖括号，转换成 html 格式，安装 xss

#### 密码加密

万一数据库被攻破，最不应该泄露的就是用户信息

攻击方式：获取用户名和密码，再去尝试登录其他系统

预防措施：进行密码加密，致使拿到密码也不知道明文

使用 cyrpto，nodejs 内置的加密包
