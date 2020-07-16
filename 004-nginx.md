# nginx

## 安装nginx
### 编译nginx

- 下载地址：http://nginx.org/en/download.html
  - ![image-20200427204132055](pic_lib/image-20200427204132055.png)


#### 介绍各目录

##### 源码目录

- ![image-20200430150938491](pic_lib/image-20200430150938491.png)
	- auto
		- ![image-20200430141935725](pic_lib/image-20200430141935725.png)
	-  CHANGE 文件 - 版本变化说明
	- conf - 配置示例文件夹
		- ![image-20200430145834188](pic_lib/image-20200430145834188.png)
	- configure 脚本-编译必备文件
	- contrib 文件夹 
		- nginx vim语法文件
	- html 文件夹
		- 提供两个标准的html文件
		- ![image-20200430151053701](pic_lib/image-20200430151053701.png)
		- 500错误的重定向文件，和默认index.html访问文件
	- man 文件夹
		- linux对nginx 的帮助文件
	- src 目录
		- nginx的源代码

##### Configure

- ![image-20200430151506565](pic_lib/image-20200430151506565.png)
	- 定义一些路径，编译安装去这些文件找相应功能
	- `--prefix=PATH ` nginx 安装路径
- ![image-20200430151640276](pic_lib/image-20200430151640276.png)
	- 默认使用和不使用某些模块
		- with 默认不使用，如果使用，需要编译的时候加上参数
		- without 默认使用，如果不使用，编译的时候加上参数
- ![image-20200430151832468](pic_lib/image-20200430151832468.png)
	- 编译中使用特殊的参数 

####  编译

- 执行configure文件
	- ./configure --prefix=/root/arno/nginx` 命令
		- 安装nginx时依赖库：zlib，pcre，openssl
			- `sudo apt-get install openssl libssl-dev`
			- `sudo apt-get install libpcre3 libpcre3-dev`
			- `sudo apt-get install zlib1g-dev`
	- ![image-20200430153450007](../image-20200430153450007.png)
		- nginx 配置特性和安装目录
	- 执行完成后会生成一些中间文件
		- ![image-20200430153630720](pic_lib/image-20200430153630720.png)
		- objs文件夹
			- ![image-20200430153706045](pic_lib/image-20200430153706045.png)
			- ngx_modules.c 文件
				- 决定编译时候，有哪些模块会被编译进去

### 安装

- 命令：
	- `make`
	- `maike install`

## nginx执行

- 命令：

	- `nginx -s relaod` 重新加载nginx命令
- 参数：

	- -?,-h     :帮助
	- -v   -V  打印nginx版本信息
	- -t   -T  测试配置文件是否有语法错误
	- -s  发送信号
		- stop 立即停止服务
		- quit 优雅停止服务
		- reload 重新加载配置文件
		- reopen 重新开始记录日志文件
	- -p 制定运行目录
	- -c 使用制定配置文件

## nginx 和 apache 比较优缺点

### nginx相对于apache的优点

- 轻量级，同样起web 服务，比apache 占用更少的内存及资源 
- 高并发，nginx 处理请求是异步非阻塞的，而apache 则是阻塞型的，在高并发下nginx 能保持低资源低消耗高性能 
- 高度模块化的设计，编写模块相对简单 
- 社区活跃，各种高性能模块出品迅速啊 

### apache 相对于nginx 的优点

- rewrite ，比nginx 的rewrite 强大 （地址重定向）
- 模块超多，基本想到的都可以找到 
- 少bug ，nginx 的bug 相对较多 
- 超稳定 

### web 服务器比较

- nginx 高并发 适合做http代理
- Nginx作为负载均衡服务器
- nginx 静态网页处理性能比 Apache 高
- Apache 的组件比 Nginx 多 
- nginx处理动态请求是鸡肋，一般动态请求要apache去做，nginx只适合静态和反向









