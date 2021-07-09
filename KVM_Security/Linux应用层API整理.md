# `Linux`应用层`API`汇总

## `Linux`基础`API`相关接口

```c
//打开文件夹路径
DIR *opendir (const char *__name);
//读取路径信息
struct dirent *readdir (DIR *__dirp);
//关闭文件夹
int closedir (DIR *__dirp);
//创建文件夹
int mkdir (const char *__path, __mode_t __mode);
//删除文件夹
int rmdir (const char *__path);
//获取进程ID
__pid_t getpid (void);
//获取组ID
__uid_t getuid (void);
//输出错误信息
void perror (const char *__s);
//获取错误码对应的字符串信息
char *strerror (int __errnum);
//解析命令行字符串并处理
int getopt (int ___argc, char *const *___argv, const char *__shortopts);
//获取当前时间信息(单位秒)
time_t time (time_t *__timer);
//获取文件描述符的属性信息
int __cdecl fstat (int _Desc, struct stat *_Stat);   
//获取文件的内部属性信息
int __cdecl stat (const char *_Filename, struct stat *_Stat); 
//设置文件的读写权限                    
int chmod (const char *__file, __mode_t __mode);      
//根据用户名获得用户信息            
struct passwd *getpwnam (const char *__name);
int getpwnam_r (const char *__restrict __name,
		       struct passwd *__restrict __resultbuf,
		       char *__restrict __buffer, size_t __buflen,
		       struct passwd **__restrict __result);
//根据UID获得用户信息     
struct passwd *getpwuid (__uid_t __uid);
int getpwuid_r (__uid_t __uid,
		       struct passwd *__restrict __resultbuf,
		       char *__restrict __buffer, size_t __buflen,
		       struct passwd **__restrict __result);  
//获得系统参数的值
long int sysconf (int __name);          
//根据用户名获得用户组信息       
struct group *getgrnam (const char *__name);
int getgrnam_r (const char *__restrict __name,
		       struct group *__restrict __resultbuf,
		       char *__restrict __buffer, size_t __buflen,
		       struct group **__restrict __result);
//根据UID获得用户组信息     
struct group *getgrgid (__gid_t __gid);
int getgrgid_r (__gid_t __gid, 
               struct group *__restrict __resultbuf,
		       char *__restrict __buffer, size_t __buflen,
		       struct group **__restrict __result);    
//获取当前用户对文件的读写权限,或者判断存在 
int access (const char *__name, int __type);    
//扩展或者截断文件             
int truncate (const char *__file, __off_t __length);           
//创建文件的链接   
int link (const char *__from, const char *__to);     
//创建文件的软链接                
int symlink (const char *__from, const char *__to); 
//找到软链接对应的实际文件，并获得名称
ssize_t readlink (const char *__restrict __path,
                  char *__restrict __buf, size_t __len);
//删除链接或者文件               
int unlink (const char *__name);  
//改变文件的所属用户和所属组              
int chown (const char *__file, __uid_t __owner, __gid_t __group);    
//获取当前的文件路径              
char *getcwd (char *__buf, size_t __size);
```

## 字符相关处理的`API`接口

```c
//判断是否为字母或数字字符
int isalnum(int); 
//判断是否为ASCII码, 宏定义
#define isascii
//判断是否为英文字母
int isalpha(int); 
//判断是否为ASCII码的控制字符
int iscntrl(int);
//判断字符是否为阿拉伯数字
int isdigit(int); 
//判断字符是否可图形显示
int isgraph(int); 
//判断字符是否可打印
int isprint(int); 
//判断是否为小写字母
int islower(int); 
//判断是否为大写字母
int isupper(int); 
//判断是否为空格字符
int isspace(int); 
//判断是否为标点符号
int ispunct(int); 
//判断是否符合十六进制的字符(0~f)
int isxdigit(int);

//字符串转double类型
double atof(const char *__nptr); 
//字符串转int类型   
int atoi(const char *__nptr);  
//字符串转长整型
int atol(const char *__nptr);     

//浮点型转字符串
extern char *gcvt (double __value, int __ndigit, char *__buf);
//字符串转dobule型，带返回终止字符  
double __cdecl __MINGW_NOTHROW strtod(const char * __restrict__ _Str, char ** __restrict__ _EndPtr);
//字符串long型，带返回终止字符
long __cdecl __MINGW_NOTHROW strtol(const char * __restrict__ _Str, char ** __restrict__ _EndPtr) 
//字符串long long型，带返回终止字符
long long __cdecl __MINGW_NOTHROW strtoll(const char * __restrict__ _Str, char ** __restrict__ _EndPtr)
//字符串unsigned long型，带返回终止字符
unsigned long __cdecl __MINGW_NOTHROW strtoul(const char * __restrict__ _Str, char ** __restrict__ _EndPtr)
//字符串unsigned long long型，带返回终止字符
unsigned long long __cdecl __MINGW_NOTHROW strtoull(const char * __restrict__ _Str, char ** __restrict__ _EndPtr)
```

## `Linux`命名先入先出管道通讯

```c
//用于创建fifo管道的应用实现
int mkfifo (const char *__path, __mode_t __mode);
//打开FIFO管道,获取后续使用的描述符
int open (const char *pathname, int oflag,...);  
//从FIFO管道中读取数据  
ssize_t read (int fd, void * buf, size_t count);  
//向FIFO管道写入数据  
ssize_t write (int fd, const void * buf, size_t count);  
//关闭FIFO管道
int close (int fd);  
//移除FIFO管道
int unlink (const char *__name);  
```

## `Linux`文件`I/O`操作

```c
//打开文件,获得文件描述符
int open(const char *pathname, int oflag,...);  
//从文件中读取数据  
ssize_t read(int fd, void * buf, size_t count);  
//向文件中写入数据  
ssize_t write(int fd, const void * buf, size_t count);  
//关闭文件描述符  
int close(int fd);  
//移动文件指针偏移地址,和read配合使用从指定起始地址读取数据  
off_t lseek(int fildes, off_t offset, int whence);
//从文件读入数据到缓冲数组中
ssize_t readv(int fd, const struct iovec *iov, int iovcnt);
//将缓冲数组里的数据写入文件
ssize_t writev(int fd, const struct iovec *iov, int iovcnt);
//对文件随机读/写
ssize_t pread(int fd, void *buf, size_t count, off_t offset);
ssize_t pwrite(int fd, const void *buf, size_t count, off_t offset);
//在64位地址空间里移动文件指针
int _llseek(unsigned int fd, unsigned long offset_high,  unsigned long offset_low, loff_t *result,  unsigned int whence);
//I/O多路转换
int poll(struct pollfd *fds, nfds_t nfds, int timeout);
//获取文件系统信息
int statfs(const char *path, struct statfs *buf); 
int fstatfs(int fd, struct statfs *buf);
//读取目录项
int getdents(unsigned int fd, struct dirent *dirp, unsigned int count);
//创建一个特殊的文件或普通文件
int mknod(const char *pathname, mode_t mode, dev_t dev);
//操作磁盘配额
long quotactl(int cmd, char *special, qid_t id, caddr_t addr)
```

## 进程的创建和应用

```c
//创建
//系统调用创建和原进程资源一致的的新进程(会复制资源重新分配)
pid_t fork(void);
//vfork保证子进程先运行，父进程会被阻塞直到子进程调用exec或exit之后，父进程才可能被调度运行
pid_t vfork(void);
//进程克隆，可以将父进程资源有选择地复制给子进程，而没有复制的数据结构则通过指针的复制让子进程共享
int clone(int (*fn)(void *), void *child_stack, int flags, void *arg);
//获得当前进程的pid值
pid_t getpid(void);
//获得当前进程的父进程pid值
pid_t getppid(void);
//运行可执行文件，系统调用
int execve(const char *path, char *const argv[], char *const envp[]);
//中止进程，_exit函数是系统调用，而exit函数是库函数
void exit(int status);
void _exit(int status);
//进程所能打开的最大文件数
int getdtablesize(void);
//获取当前进程的进程组ID
pid_t getpgrp(void);
//设置当前进程组标志号
int setpgrp(void);
//设置指定进程组标志号
int setpgid(pid_t pid, pid_t pgid);
//获取指定进程的进程组ID
pid_t getpgid(pid_t pid);
//读写进程的本地描述表
int modify_ldt(int func, void *ptr, unsigned long bytecount);
//使进程睡眠指定的时间
int nanosleep(const struct timespec *req, struct timespec *rem);
//挂起进程，等待信号
int pause(void);
//设置进程运行域
int personality(unsigned long persona);
//对进程进行特定操作
int prctl(int option, unsigned long arg2, unsigned long arg3, unsigned long arg4, unsigned long arg5);
//进程跟踪
long ptrace(enum __ptrace_request request, pid_t pid, void *addr, void *data);
//获取静态优先级的上下限
int sched_get_priority_max(int policy);
int sched_get_priority_min(int policy);
//获取进程的调度参数
int sched_getparam(pid_t pid, struct sched_param *param);
//设置进程的调度参数
int sched_setparam(pid_t pid, const struct sched_param *param);
//获取指定进程的调度策略
int sched_getscheduler(pid_t pid);
//设置指定进程的调度策略和参数
int sched_setscheduler(pid_t pid, int policy, const struct sched_param *param);
//获取按RR算法调度的实时进程的时间片长度
int sched_rr_get_interval(pid_t pid, struct timespec *tp);
//进程主动让出处理器,并将自己等候调度队列队尾
int sched_yield(void);
//获取/设置进程权限
int capget(cap_user_header_t hdrp, cap_user_data_t datap);
int capset(cap_user_header_t hdrp, const cap_user_data_t datap);
//获取/设置会晤标识号
pid_t getsid(pid_t pid);
pid_t setsid(void);
```

## `Linux`进程间消息队列处理

```c
//创建消息队列
int msgget(key_t key, int oflg);
//从消息队列里读取数据
ssize_t msgrcv(int msqid, void *ptr, size_t length, long type, int flag);
//创建一个新的消息队列或访问一个已存在的消息队列
int msgsnd(int msqid, const void *ptr, size_t length, int flag);
//提供在一个消息队列上的各种控制操作
int msgctl(int msqid, int cmd, struct msqid_ds *buff);
```

## `Linux`多线程接口

```c
//线程的创建  
int pthread_create(pthread_t *tid, const pthread_arrt_t* attr, void*(*start_routine)(void *), void* arg);  
//等待线程的结束,非分离的线程在结束后只有执行join才会释放全部资源  
int pthread_join(pthread_t thread, void **retval);
//线程离开时的返回值(必须为malloc或者全局变量)  
void pthread_exit(void * retval);
//分离线程,被分离的线程在结束后自动释放所有资源  
int pthread_detach(pthread_t tid);
//多线程的交互--互斥量和自旋锁  
//互斥量 
//互斥量申请
int pthread_mutex_init(pthread_mutex_t *m, const pthread_mutexattr_t *a);
//互斥量加锁
int pthread_mutex_lock(pthread_mutex_t *m);
//互斥量释放解锁
int pthread_mutex_unlock(pthread_mutex_t *m);
//互斥量尝试加锁，非堵塞
int pthread_mutex_trylock(pthread_mutex_t *m);
//互斥量释放
int pthread_mutex_destroy(pthread_mutex_t *m); 
//自旋锁  
pthread_spinlock_t m_spinlock;  
//自旋锁申请
int pthread_spin_init(pthread_spinlock_t *lock, int pshared);
//自旋锁加锁
int pthread_spin_lock(pthread_spinlock_t  *m_spinlock);  
//自旋锁释放
int pthread_spin_unlock(pthread_spinlock_t  *m_spinlock);  
//自旋锁尝试加锁，非堵塞
int pthread_spin_trylock(pthread_spinlock_t  *m_spinlock); 
//自旋锁释放
int pthread_spin_destroy(pthread_spinlock_t  *m_spinlock); 
```

## 用于进程间通讯的管道

```c
//创建pipe通道, 其中fd[0]为数据读管道描述符，fd[1]为数据写管道描述符
int pipe(int fd[2]);
//通过管道描述符从管道中读取数据  
ssize_t read(int fd, void * buf, size_t count);  
//通过管道描述符向管道中写入数据
ssize_t write (int fd, const void *buf, size_t count);  
//关闭通道的接口应用
int close(int fd);
```

## 用于进程间通讯的信号量

```c
//信号量控制
int semctl(int semid, int semnum, int cmd, ...);
//获取一组信号量
int semget(key_t key, int nsems, int semflg);
//信号量操作
int semop(int semid, struct sembuf *sops, size_t nsops);
```

## 用于进程间通讯的共享内存

```c
//控制共享内存
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
//获取共享内存
int shmget(key_t key, size_t size, int shmflg);
//连接共享内存
void *shmat(int shm_id, const void *shm_addr, int shmflg);
//拆卸共享内存
int shmdt(const void *shmaddr);
```

## 基于`posix`接口的消息队列

```c
//删除已经存在的消息队列
int mq_unlink(const char *name);    
//打开或者创建一个消息队列
mqd_t mq_open(const char *name, int oflag, ...);
//关闭消息队列  
int mq_close(mqd_t mqdes);
//获取消息队列的具体参数信息
int mq_getattr(mqd_t mqdes, struct mq_attr *attr);
//投递数据给消息队列
int mq_send(mqd_t mqdes, const char *ptr, size_t len, unsigned int prio);
//等待消息队列有消息接收
ssize_t mq_receive(mqd_t mqdes, const char *ptr, size_t len, unsigned int *prio); 
```

## 系统环境变量访问

```c
//获取系统环境变量
char *getenv(const char *name);
//添加和修改环境变量
int setenv(const char *name, const char *value, int rewrite);
//删除环境变量
int unsetenv(const char *name);
//写入环境变量，覆盖或者添加, 和setenv功能类似，实现上格式为name=string
char *putenv(char *str); 
//执行调用其它程序或者指令
int system(const char *__command);
```

## `TCP`客户端和服务器

```c
//TCP客户端接口
//创建网络套接字    
int socket(int domain, int type, int protocol)    
//TCP握手连接到指定IP地址和端口  
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);   
//向连接的服务器通过TCP发送数据  
ssize_t write(int fd, const void *buf, size_t count);  
//从连接的服务器读取TCP数据  
ssize_t read(int fd, void *buf, size_t count);  
//关闭Socke通讯
int close(int fd);
//TCP服务器接口  
//在包含上述客户端接口外，额外需要服务器绑定的接口和等待连接的接口  
//TCP服务器绑定到指定的IP地址和客户端  
int bind(int sockfd, const struct sockaddr* my_addr, socklen_t addrlen);  
//TCP等待客户端的连接  
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);  
```

## `Linux`软件定时器和信号触发

```c
//用于连接信号和处理函数的实现
sighandler_t signal(int signum, sighandler_t handler);
//创建软件定时器的函数
int setitimer(int which, const struct itimerval *value, struct itimerval *ovalue));
```

## 系统时间处理的接口

```c
//获取系统的日历时间，以1970-1-1, 00:00:00开始
time_t time (time_t *__timer);
//根据日历时间获取GMT时间的函数
struct tm *gmtime (const time_t *__timer);
struct tm *gmtime_r (const time_t *__restrict __timer,  struct tm *__restrict __tp)
//获取本地时间的函数
struct tm *localtime (const time_t *__timer);
struct tm *localtime_r (const time_t *__restrict __timer, struct tm *__restrict __tp)
//根据本地时间生成时间字符串
char *asctime (const struct tm *__tp);
char *asctime_r (const struct tm *__restrict __tp, char *__restrict __buf);
//根据日历时间生成时间字符串
char *ctime (const time_t *__timer);
char *ctime_r (const time_t *__restrict __timer, char *__restrict __buf);
//将带时区的时间转换成秒数
time_t mktime (struct tm *__tp);
```

## `UDP`客户端和服务器

```c
//UDP客户端接口
//创建网络套接字  
int socket(int domain, int type, int protocol)  
//UDP数据发送  
ssize_t sendto(int sockfd, const void *buf, size_t len, int flags, const struct sockaddr *dest_addr, socklen_t addrlen);   
//UDP数据接收  
ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr *src_addr, socklen_t *addrlen);  
//关闭Socket通讯  
int close(int fd); 
//UDP服务器接口  
//在包含上述客户端接口外，额外需要服务器绑定的接口  
//UDP服务器绑定到指定的IP地址和客户端  
int bind(int sockfd, const struct sockaddr* my_addr, socklen_t addrlen);  
```

## 系统控制

```c
//I/O总控制函数
int ioctl(int d, int request, ...);
//读/写系统参数
int _sysctl(struct __sysctl_args *args);
//启用或禁止进程记账
int acct(const char *filename);
//获取/设置系统资源上限
int getrlimit(int resource, struct rlimit *rlim); 
int setrlimit(int resource, const struct rlimit *rlim);
//获取系统资源使用情况
int getrusage(int who, struct rusage *usage);
//选择要使用的二进制函数库
int uselib(const char *library);
//设置端口I/O权限
int ioperm(unsigned long from, unsigned long num, int turn_on);
//改变进程I/O权限级别
int iopl(int level);
//打开/关闭交换文件和设备
int swapon(const char *path, int swapflags);
int swapoff(const char *path);
//控制bdflush守护进程
int bdflush(int func, long *address); int bdflush(int func, long data);
//对NFS守护进程进行控制
nfsservctl(int cmd, struct nfsctl_arg *argp, union nfsctl_res *resp);
//创建可装载的模块项
caddr_t create_module(const char *name, size_t size);
//删除可装载的模块项
int delete_module(const char *name, int flags);
//初始化模块
int init_module(void *module_image, unsigned long len,
                const char *param_values);
//查询模块信息
int query_module(const char *name, int which, void *buf,
                        size_t bufsize, size_t *ret);
```

## 内存管理

```c
//改变数据段空间的分配
int brk(void *end_data_segment);
void *sbrk(intptr_t increment);
//内存页面加/解锁
int mlock(const void *addr, size_t len); 
int munlock(const void *addr, size_t len); 
int mlockall(int flags); 
int munlockall(void);
//映射/取消映射虚拟内存页
void *mmap(void *start, size_t length, int prot, int flags,  int fd, off_t offset);
int munmap(void *start, size_t length);
//重新映射虚拟内存地址
void * mremap(void *old_address, size_t old_size , size_t new_size, int flags);
//将映射内存中的数据写回磁盘
int msync(void *start, size_t length, int flags);
//设置内存映像保护
int mprotect(const void *addr, size_t len, int prot);
//获取页面大小
int getpagesize(void);
//将内存缓冲区数据写回硬盘
void sync(void);
//将指定缓冲区中的内容写回磁盘
int cacheflush(char *addr, int nbytes, int cache); 
```

## 网络管理

```c
//获取/设置域名
int getdomainname(char *name, size_t len); 
int setdomainname(const char *name, size_t len);
//获取/设置主机标识号
long gethostid(void); 
int sethostid(long hostid);
//获取/设置本主机名称
int gethostname(char *name, size_t len); 
int sethostname(const char *name, size_t len);
```



## 附录

https://www.kancloud.cn/wizardforcel/linux-c-api-ref/98327
