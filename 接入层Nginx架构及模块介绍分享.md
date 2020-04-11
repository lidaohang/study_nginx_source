#### 课程目标：
1）帮助大家对Nginx有一定的认识
2）熟悉Nginx有哪些应用场景
3）熟悉Nginx特点和架构模型以及相关流程
4）熟悉Nginx定制化开发的几种模块分类

#### 课程大纲：
1. Nginx简介及特点
2. Nginx应用场景
3. Nginx框架模型介绍
4. Nginx内部流程介绍
5. Nginx自定义模块开发介绍
6. Nginx核心时间点模块介绍
7. Nginx分流模块介绍
8. Nginx动态upstream模块介绍
9. Nginx query_upstrem模块介绍
10. Nginx query_conf模块介绍
11. Nginx 共享内存支持redis协议模块介绍
12. Nginx 日志回放压测工具介绍

#1. Nginx简介以及特点
**Nginx简介:**

Nginx (engine x) 是一个高性能的web服务器和反向代理服务器，也是一个IMAP/POP3/SMTP服务器
- 俄罗斯程序员Igor Sysoev于2002年开始 
- Nginx是增长最快的Web服务器，市场份额已达33.3％
- 全球使用量排名第二2011年成立商业公司

**Nginx社区分支：**
- Openresty作者@agentzh(章宜春)开发的，最大特点是引入了ngx_lua模块，支持使用lua开发插件，并且集合了很多丰富的模块，以及lua库。
- Tengine主要是淘宝团队开发。特点是融入了因淘宝自身的一些业务带来的新功能。
- Nginx官方版本，更新迭代比较快，并且提供免费版本和商业版本。

**Nginx源码结构：**

- 代码量大约11万行C代码
- 源代码目录结构
  - core （主干和基础设置）
  - event （事件驱动模型和不同的IO复用模块）
  - http （HTTP服务器和模块）
  - mail （邮件代理服务器和模块）
  - os （操作系统相关的实现）
  - misc （杂项）

**Nginx特点：**

- 反向代理，负载均衡器
- 高可靠性、单master多worker模式
- 高可扩展性、高度模块化
- 非阻塞
- 事件驱动
- 低内存消耗
- 热部署

# 2. Nginx应用场景

**场景如下：**

- 静态文件服务器
- 反向代理，负载均衡
- 安全防御
- 智能路由
- 灰度发布
- 静态化
- 消息推送
- 图片实时压缩
- 防盗链

# 3. Nginx框架模型介绍

**进程组件角色：** 

- master进程
  - 监视工作进程的状态
  - 当工作进程死掉后重启一个新的
  - 处理信号和通知工作进程
- worker进程
  - 处理客户端请求
  - 从主进程处获得信号做相应的事情
- cache loader进程
  - 加载缓存索引文件信息，然后退出
- cache manager进程
  - 管理磁盘的缓存大小，超过预定值大小后最少使用数据将被删除

**框架模型：**
![image.png](https://upload-images.jianshu.io/upload_images/2099201-0bf59b45444dc96e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**框架模型流程：**
![image.png](https://upload-images.jianshu.io/upload_images/2099201-4b658647aa125a1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 4. Nginx内部流程介绍
## 4.1 框架模型流程
![image.png](https://upload-images.jianshu.io/upload_images/2099201-fb679fb4e47b2818.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/2099201-2ea2ee9bf2895042.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.2 master初始化流程
![master初始化流程.png](https://upload-images.jianshu.io/upload_images/2099201-bc25f127d3fd0d1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.3 worker初始化
![image.png](https://upload-images.jianshu.io/upload_images/2099201-f01d986182c84e82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.4 worker初始化流程
![worker进程初始化流程.png](https://upload-images.jianshu.io/upload_images/2099201-bcd7e52338e84bdd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.5 静态文件请求IO流程
![image.png](https://upload-images.jianshu.io/upload_images/2099201-b5005a213bef2090.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.6 http请求流程
![HTTP请求流程.png](https://upload-images.jianshu.io/upload_images/2099201-39041753c9c77d50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.7 http请求11个阶段
![image.png](https://upload-images.jianshu.io/upload_images/2099201-cceca7333bb7f5d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.8 upstream模块

- 访问第三方Server服务器
- 底层HTTP通信非常完善
- 异步非阻塞
- 上下游内存零拷贝，节省内存
- 支持自定义模块开发

### 4.8.1 upstream框架流程
![image.png](https://upload-images.jianshu.io/upload_images/2099201-d8254e0481b8a507.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4.8.2 upstream内部流程
![upstream流程.png](https://upload-images.jianshu.io/upload_images/2099201-dfe91248b0938d74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 4.9 反向代理流程
![image.png](https://upload-images.jianshu.io/upload_images/2099201-6bf1a1e1f1ce0001.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 5. Nginx定制化模块开发

## 5.1 Nginx的模块化设计特点

- 高度抽象的模块接口
- 模块接口非常简单，具有很高的灵活性
- 配置模块的设计
- 核心模块接口的简单化
- 多层次、多类别的模块设计

## 5.1 内部核心模块
![image.png](https://upload-images.jianshu.io/upload_images/2099201-8cb7b6914b40ed2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Nginx核心模块.png](https://upload-images.jianshu.io/upload_images/2099201-7173c5a937548851.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 5.2 handler模块
- 接受来自客户端的请求并构建响应头和响应体。    
![handler.png](https://upload-images.jianshu.io/upload_images/2099201-dd88eaaf2479f087.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 5.3 filter模块
- 过滤（filter）模块是过滤响应头和内容的模块，可以对回复的头和内容进行处理。它的处理时间在获取回复内容之后，向用户发送响应之前。
![filter.png](https://upload-images.jianshu.io/upload_images/2099201-d8db7067b3a44ec8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

   
## 5.4 upstream模块
- 使nginx跨越单机的限制，完成网络数据的接收、处理和转发，纯异步的访问后端服务。
![upstream.png](https://upload-images.jianshu.io/upload_images/2099201-b9b6252f37a574e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**load_balance：**
- 负载均衡模块，实现特定的算法，在众多的后端服务器中，选择一个服务器出来作为某个请求的转发服务器。
![load_balabce.png](https://upload-images.jianshu.io/upload_images/2099201-e5794e5713b82014.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 5.3 ngx_lua模块

- 脚本语言
- 内存开销小
- 运行速度快
- 强大的 Lua 协程
- 非阻塞
- 业务逻辑以自然逻辑书写

![ngx_lua_phase.png.jpg](https://upload-images.jianshu.io/upload_images/2099201-f029d1c935c696d5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 5.4 定制化开发Demo
**Handler模块：**
- 编写config文件
- 编写模块产生内容响应信息

```
#配置文件：
server {
    ...    
    location test {
        test_counter on;
    }
}
#config
ngx_addon_name=ngx_http_test_module
HTTP_MODULES="$HTTP_MODULES ngx_http_test_module"
NGX_ADDON_SRCS="$NGX_ADDON_SRCS $ngx_addon_dir/ngx_http_test_module.c"
#ngx_http_test_module.c
static ngx_int_t
ngx_http_test_handler(ngx_http_request_t *r)
{
	ngx_int_t								rc;
	ngx_buf_t								*b;
	ngx_chain_t								out;
	ngx_http_test_conf_t				    *lrcf;
	ngx_str_t								ngx_test_string = ngx_string("hello test");

	lrcf = ngx_http_get_module_loc_conf(r, ngx_http_test_module);
	if ( lrcf->test_counter == 0 ) {
		return NGX_DECLINED;
	}
	
	/* we response to 'GET' and 'HEAD' requests only */
	if ( !(r->method & (NGX_HTTP_GET|NGX_HTTP_HEAD)) ) {
			return NGX_HTTP_NOT_ALLOWED;
	}

	/* discard request body, since we don't need it here */
	rc = ngx_http_discard_request_body(r);

	if ( rc != NGX_OK ) {
		return rc;
	}
	
	/* set the 'Content-type' header */
	/*
	 *r->headers_out.content_type.len = sizeof("text/html") - 1;
	 *r->headers_out.content_type.data = (u_char *)"text/html";
    */
	ngx_str_set(&r->headers_out.content_type, "text/html");

	/* send the header only, if the request type is http 'HEAD' */
	if ( r->method == NGX_HTTP_HEAD ) {
		r->headers_out.status = NGX_HTTP_OK;
		r->headers_out.content_length_n = ngx_test_string.len;

		return ngx_http_send_header(r);
	}
	
	/* set the status line */
	r->headers_out.status = NGX_HTTP_OK;
	r->headers_out.content_length_n =  ngx_test_string.len;

	/* send the headers of your response */
	rc = ngx_http_send_header(r);
	if ( rc == NGX_ERROR || rc > NGX_OK || r->header_only ) {
		return rc;
	}

	/* allocate a buffer for your response body */
	b = ngx_pcalloc(r->pool, sizeof(ngx_buf_t));
	if ( b == NULL ) {
		return NGX_HTTP_INTERNAL_SERVER_ERROR;
	}
	
	/* attach this buffer to the buffer chain */
	out.buf = b;
	out.next = NULL;

	/* adjust the pointers of the buffer */
	b->pos = ngx_test_string.data;
	b->last = ngx_test_string.data + ngx_test_string.len;
	b->memory = 1;    /* this buffer is in memory */
	b->last_buf = 1;  /* this is the last buffer in the buffer chain */
	
	/* send the buffer chain of your response */
	return ngx_http_output_filter(r, &out);
}
```
# 6. Nginx核心时间点模块介绍
解决接入层故障定位慢的问题，帮助OP快速判定问题根因，优先自证清白，提高接入层高效的生产力。
![image.png](https://upload-images.jianshu.io/upload_images/2099201-9c3e50030523dbb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 7. Nginx分流模块介绍
**特点：**
实现非常灵活的动态的修改策略从而进行切流量。
实现平滑无损的方式进行流量的切换。
通过秒级切换流量可以缩小影响范围，从而减少损失。
按照某一城市或者某个特征，秒级进行切换流量或者禁用流量。
容忍单机房级别容量故障，缩短了单机房故障的止损时间。
快速的将流量隔离或者流量抽样。
高效的灰度测试，提高生产力。
![image.png](https://upload-images.jianshu.io/upload_images/2099201-a15479be8aaea41a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 8. Nginx动态upstream模块介绍
让接入层可以适配动态调度的云环境，实现服务的平滑上下线、弹性扩/缩容。
从而提高接入层高效的生产力以及稳定性，保证业务流量的平滑无损。
![image.png](https://upload-images.jianshu.io/upload_images/2099201-c960757e90abde41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 9. Nginx query_upstream模块介绍
链路追踪，梳理接口到后端链路的情况。查询location接口对应upstream server信息。
![image.png](https://upload-images.jianshu.io/upload_images/2099201-c9834ca1f26f60c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 10. Nginx query_conf模块介绍
获取nginx配置文件格式化为json格式信息。
![image.png](https://upload-images.jianshu.io/upload_images/2099201-3089ecc028a01852.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 11.Nginx 共享内存支持redis协议模块介绍
根据配置文件来动态的添加共享内存。
https://github.com/lidaohang/ngx_shm_dict
- ngx_shm_dict
  共享内存核心模块(红黑树，队列)

- ngx_shm_dict_manager
  添加定时器事件，定时的清除共享内存中过期的key
  添加读事件，支持redis协议，通过redis-cli get,set,del,ttl

- ngx_shm_dict_view
  共享内存查看
![image.png](https://upload-images.jianshu.io/upload_images/2099201-eff270d5cc6a3400.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 12. Nginx日志回放压测工具
*   解析日志进行回放压测，模拟后端服务器慢等各种异常情况
[https://github.com/lidaohang/playback-testing](https://github.com/lidaohang/playback-testing)


#### [](https://github.com/lidaohang/playback-testing#%E6%96%B9%E6%A1%88%E8%AF%B4%E6%98%8E)方案说明

*   客户端解析access.log构建请求的host,port,url,body
*   把后端响应时间，后端响应状态码，后端响应大小放入header头中
*   后端服务器获取相应的header，进行模拟响应body大小，响应状态码，响应时间

#### [](https://github.com/lidaohang/playback-testing#%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F)使用方式

*   拷贝需要测试的access.log的日志到logs文件夹里面
*   搭建需要测试的nginx服务器，并且配置upstream 指向后端服务器断端口
*   启动后端服务器实例 server/backserver/main.go
*   进行压测 bin/wrk -c30 -t1 -s conf/nginx_log.lua [http://localhost:8095](http://localhost:8095/)
