标题：#
# 标题

## 无序列表： * + -
* 列表1

## 有序列表：数字.
1. 有序列表

## 区块引用： > >>
> 引用1
>> 引用2

## 分割线：***或---或___
***
---
___

## 链接
## 行内式： [文字]（链接） 
##      参数式： 直接放链接 或者 [参数名]:链接
### 参数式
 [test1]:http://example.com/
[test2]:http://example.com/

先验证[test1],再验证[test2]
### 行内式
 [百度](http://www.baidu.com)


## 图片 和链接几乎一样 前有!
### 行内式
![图片说明](https://images2015.cnblogs.com/blog/600165/201701/600165-20170121185054312-549083784.png)
### 参数式
[图片说明]:(https://images2015.cnblogs.com/blog/600165/201701/600165-20170121185054312-549083784.png)
参数式图片，这里是![图片说明]

## 代码框 
### 第一种 单行用` `
`http A B C D`
### 第二种 多行用三个 ``` ```
``` 可以写注释
abcd
```

## 表格
| name | sex | age |
|-|-|-|
|jack|man|23|
|lucy|wuman|19

## 强调 一个*或_包裹为倾斜，两个为加粗
*倾斜* _倾斜_
**加粗** __加粗__

## 转义 \加符号

## 删除线 ~~内容~~

~~删掉~~ 


# Tinyhttpd
## 状态码
* 400：客户端请求的语法错误，服务器无法理解 解析错误
* 404：服务器无法根据客户端的请求找到资源
* 500：服务器内部错误，无法完成请求
* 501：服务器不支持请求的功能  
  
  ***

## 进程（程序）
区分：程序是静态的，怎么执行，进程是程序的具体执行。
1. 进程id
   pid_t pid; \\pid进程id每个进程都有一个唯一的id标识进程类型是int
2. fork()
   pid = fork(); \\叉子函数创造子进程，父子进程除了进程id不一样，别的都一样  
```
            ___>pid = 0 子进程  
pid=fork()_|  
           |___>pid > 0 父进程  
```

3. exec 函数
    int execl(const char *path,const char *arg0,...)    
    path:程序的路径 /htdoc/test.cgi  
    execl("/htdoc/test.cgi","/htdoc/test.cgi",NULL)

4. waitpid() 函数  
父进程阻塞等待子进程执行结束，清理子进程，如果不收尸则子进程结束后变成僵尸进程占用系统资源    
pid_t  waitpid(pid_t pid,int *stat_loc,int options);  

***
## 线程（函数）

1. 线程id
     pthread_t tid;
     进程中可以包含多个线程，线程id在特定的进程中唯一。
2. pthread_create()创建线程  
int pthread_create(thread_t *thread,//创建的线程id   
const pthread_attr_t *attr, //属性设置  
void *(*start)(void *), //线程要执行的函数  
void *arg); 传递给函数的参数

例:  
 void * accept_request(void *);  
 pthread_t newthread;
 pthread_create(&newthread,  
 NULL,  
 accept_request,  
 (void*)10)

 3. pthread_self()  
    某个线程查询自己的线程id是什么  

4. void pthread_exit(void *retval);  
   线程退出  

5. pthread_detach() //线程结束后不需要清理，不会产生僵尸线程  
线程分类

***

## socket
1. socket文件描述符 int sfd;
2. 创建socket  
   int socket(int domain,int type,int protocol);    
   domain:PF_UNIX(unix域)，PF_INET(IPv4)，PF_INET6(IPv6) 
   type:指定socket类型。(TCP:SOCK_STREAM)和(UDP:SOCK_DGRAM)  
   protocol:指定协议，如不想指定，可用0  
3. 绑定ip和端口  
   int bind(int socket,//socket()创建的socket  
            const struct sockaddr *address, //地址  
            socklen_t address_len);//sizeof(address)  
   struct sockaddr 与 struct sockaddr_in 两个结构体大小相同  
   struct sockaddr{  
      sa_family_t sin_family; //地址族  
      char sa_data[14]; //14字节，包含套接字的地址和端口信息  
   }  
   struct sockaddr_in{  //分的比较详细  
      sa_family_t sin_family;//地址族(IPV4或IPV6等)  
      unit16_t sin_port; //16位两字节端口号  
      struct in_addr sin_addr; 32位4字节ip地址  
      char sin_zero[8]; //未使用 14=2+4+8  
   }  
   struct in_addr{  
      in_addr_t s_addr; //32位4字节ip地址  
   }  
   sin_port和sin_addr都必须是网络字节序(NBO),(大端)排序方式。  
   计算机显示的数字都是主机字节序(HBO)  
4. int listen(int socket,int backlog);  
   使套接字开始监听链接，并制定链接队列的大小  

5. 处理连接请求:  
      int accept(int socket,  
                 struct sockaddr *restrict address,//客户端的地址信息  
                 socklen_t *restrict address_len);  
      返回一个新的与客户端连接的套接字  

***

##pipe与CGI
  1. int filedges[2]   
     int pipe(filedes);  
     filedes[0] 读端  
     filedes[1] 写端  
   2. int dup2(int filedes,int filedes2);  
   复制文件描述符filedes到filedes2,filedes2如果存在则关闭fildes2  
   3. CGI
        

