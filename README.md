### 核心流程图


---

#### master初始化流程
![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/master.png)


#### worker初始化流程
![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/worker.png)
   
   
#### http请求流程
![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/http.png)


#### upstream流程
![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/upstream.png)


#### nginx请求11个阶段
![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/nginx_11_phase.png)


#### 核心模块
![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/core.png)



### 定制化模分类


---

#### handler模块
- 接受来自客户端的请求并构建响应头和响应体。    

![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/handler_module.png)


#### filter模块
- 过滤（filter）模块是过滤响应头和内容的模块，可以对回复的头和内容进行处理。它的处理时间在获取回复内容之后，向用户发送响应之前。

![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/filter_module.png)
   
   
#### upstream模块
- 使nginx跨越单机的限制，完成网络数据的接收、处理和转发，纯异步的访问后端服务。

![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/upstream_module.png)


#### load_balance
- 负载均衡模块，实现特定的算法，在众多的后端服务器中，选择一个服务器出来作为某个请求的转发服务器。

![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/load_balance_module.png)


#### ngx_lua模块
- 脚本语言
- 内存开销小
- 运行速度快
- 强大的 Lua 协程
- 非阻塞
- 业务逻辑以自然逻辑书写


![image](https://github.com/lidaohang/study_nginx_source/blob/master/image/ngx_lua_module.png)



