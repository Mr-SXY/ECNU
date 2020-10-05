什么时候缓存的数据会写到内核？(段错误在printf之后，不易被检查出来)
	1、遇到换行；2、遇到IO、getchar()；3、程序正常结束；4、缓存区（1K）满的时候。



### 文件I/O

**文件标识符**：

**文件状态标志**：包括只读、只写、读写等（O_WRONLY; O_RDONY; O_RDWR; O_APPEND）

可通过**文件标识符**获取**文件状态标志**，修改文件状态标志。

#### · fcntl函数

头文件：

```c
#include <unistd.h>
#include <fcntl.h>

int fcntl(int fd, int cmd, ... /* arg */ );
```

第一个参数表示<u>文件描述符</u>，第二个参数*cmd*表示<u>命令</u>，第三个参数可以是一个*long*，*int*也可以是一个结构体指针，*cmd*有5个功能：

​	（1）复制一个现存的描述符（*cmd = F_DUPFD*），新文件描述符作为函数值返回。

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>

int main()
{
	int opt = 1;
	int fd = fcntl(1, F_DUPFD, opt);	//返回一个新的文件描述符（此处为3）给fd，两个文件描述符1和3对应的文件指针都指向同一个文件表
	printf("%d\n", fd);	//fd = 3
	close(1);	//关闭文件描述符1
	
	write(3, "helloworld", 10);	//关闭文件描述符1，不影响通过文件描述符3对文件内容进行修改
}
```

​	（2）获得/设置文件描述符标记（*cmd = F_GETFD 或 F_SETFD*）

​	（3）获得/设置文件状态标志（*cmd = F_GETFL 或 F_SETFL*）

​	获得（F_GETFL）:

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>

int main()
{
	int fd = open("a.txt", O_RDWR | O_CREAT | O_APPEND);	
	
	int flag = fcntl(fd, F_GETFL);
	printf("%x\n", flag);	//flag = 8402
	if(flag & O_CREAT)	//逻辑与判断是否有该位
	{
		printf("O_CREAT\n");
	}
	if(flag & O_APPEND)
	{
		printf("O_APPEND\n");
	}
	if(flag & O_NONBLOCK)
	{
		printf("O_NONBLOCK\n");
	}
	if(flag & O_ACCMODE == O_RDONLY)	//ACCMODE = 11
	{
		printf("read\n");
	}else if(flag & O_ACCMODE == O_WRONLY)
	{
		printf("write\n");
	}else if(flag & O_ACCMODE == O_RDWR)
	{
		printf("read write\n");
	}
}
```

​	O_CREAT未输出(?)，其他正常输出。

​	设置（F_SETFL）:

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <string.h>

int main()
{
	int flag = fcntl(0, F_GETFL);
	flag |= O_NONBLOCK;	//增加标志位
	
	fcntl(0, F_SETFL, flag);
	char buf[30];
	while(1)
	{
		memset(buf, 0, sizeof(buf));
		read(0, buf, sizeof(buf));
		printf("buf:%s\n", buf);
		sleep(1);
	}
}
```

#### · 记录锁

​	包含三种状态：<u>读锁</u>，<u>写锁</u>，<u>无锁</u>，两个进程只要一个有*写锁*，其他进程无法操作。同样采用*fcntl*函数操作。

​	fcntl的*cmd*命令是<font color="#FF0000">F_GETLK</font>，<font color="#FF0000">F_SETLK</font>，<font color="#FF0000">F_SETLKW</font>(wait)，第三个参数是一个指向*flock*结构的指针。

​	*struct flock*结构体的定义：

​			short l_type;	//记录锁类型，<font color="#FF0000">F_RDLCK</font>，<font color="#FF0000">F_WRLCK</font>，<font color="#FF0000">F_UNLCK</font>。

​			off_t l_start;	//起始位置偏移量

​			short l_whence;	//SEEK_SET，SEEK_CUR，SEEK_END

​			off_t l_len;	//长度，值为0表示一直到文件尾

​			pid_t l_pid;

示例：

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>

int mian()
{
    int fd = open("a.txt", O_RDONLY);
    
    struct flock lck;
    lck.l_type = F_RDLCK;	//上读锁
    lck.l_start = 0;
    lck.l_whence = SEEK_SET;
    lck.l_len = 0;
    
    printf("start read lock\n");
    fcntl(fd, F_SETLKW, &lck);
    printf("finish read lock\n");
    
    getchar();
    
    lck.l_type = F_UNLOCK;	//解锁
    fcntl(fd, F_SETLK, &lck);
    printf("UNLOCK read lock\n");
}
```



> ​	当一个进程终止时，它所建立的锁全部被释放；当*close*一个文件描述符时，该文件上的任意一把锁都被释放。

#### ·ioctl函数

```c
#include <sys/ioctl.h>
#include <unistd.h>

int ioctl(int filedes, int cmd, ...);
```

​	与*fcntl*函数类似，第二个参数*cmd*表示要执行什么操作，一般用于操作硬件文件描述符，不同的硬件的*cmd*操作不同。（通过查询技术手册获得）

​	如下示例：摄像头操作

```c
struct video_picture v;
ioctl(fd, VIDEOCGPICT, &v);
```

### 文件与目录操作

#### ·stat函数

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>

int stat(const char *pathname, struct stat *statbuf);	//参数一文件名；参数二结构体指针
int fstat(int fd, struct stat *statbuf);	//参数一文件描述符；参数二结构体指针
int lstat(const char *pathname, struct stat *statbuf);
```

struct stat 结构体：

> struct stat
>
> {
>
> dev_t   st_dev;   */\* ID of device containing file \*/*文件使用的设备号
>
> ino_t   st_ino;   */\* inode number \*/*    索引节点号 
>
> mode_t   st_mode;   */\* protection \*/*  **文件对应的模式，文件，目录等**
>
> nlink_t  st_nlink;  */\* number of hard links \*/*    文件的硬连接数  
>
> uid_t   st_uid;   */\* user ID of owner \*/*    所有者用户识别号
>
> gid_t   st_gid;   */\* group ID of owner \*/*   组识别号  
>
> dev_t   st_rdev;   */\* device ID (if special file) \*/* 设备文件的设备号
>
> off_t   st_size;   */\* total size, in bytes \*/* 以字节为单位的文件容量  
>
> blksize_t st_blksize; */\* blocksize for file system I/O \*/* 包含该文件的磁盘块的大小  
>
> blkcnt_t  st_blocks;  */\* number of 512B blocks allocated \*/* 该文件所占的磁盘块  
>
> time_t   st_atime;  */\* time of last access \*/* 最后一次访问该文件的时间  
>
> time_t   st_mtime;  */\* time of last modification \*/* /最后一次修改该文件的时间  
>
> time_t   st_ctime;  */\* time of last status change \*/* 最后一次改变该文件状态的时间  
>
> };

stat.st_mode包含文件类型与权限信息，有以下宏：

> S_ISLNK (st_mode)   判断是否为符号连接
>
> S_ISREG (st_mode)   是否为一般文件
>
> S_ISDIR (st_mode)   是否为目录
>
> S_ISCHR (st_mode)   是否为字符装置文件
>
> S_ISBLK (s3e)     是否为先进先出
>
> S_ISSOCK (st_mode)  套接字文件

使用：

```c
struct stat t;
stat("a.txt", &t);

if(S_ISLNK(t.st_mode))	//判断是否为普通文件
{
    ......
}
```

#### ·access函数

```c
#include <unistd.h>
int access(const char *pathname, int mode);
```

*mode*为：R_OK(测试读许可); W_OK(测试写许可); X_OK(测试执行许可); F_OK(测试文件是否存在)，成功返回0，失败返回-1。

#### ·chmod函数

```c
#include <sys/stat.h>
 int chmod(const char *pathname, mode_t mode);
```

修改文件权限。

```c
chmod("a.txt", 0666);	//示例
```

#### ·link和unlink函数

```c
#include <unistd.h>
#include <stdio.h>
int link(const char *oldpath, const char *newpath);//创建硬连接
int unlink(const char *pathname);
int remove(const char *pathname);//可删除单个文件，若删除文件夹，文件夹需为空
int rename(const char *oldpath, const char *newpath);
```

#### ·mkdir和rmdir函数

```c
#include <sys/stat.h>
#include <sys/types.h>
int mkdir(const char *pathname, mode_t mode);//创建文件夹
int rmdir(const char *pathname);//删除文件夹（空）
```

#### ·opendir和readdir函数

打开文件与读文件。

```c
#include <sys/types.h>
#include <dirent.h>
DIR * dirp = opendir(const char *name);
struct dirent *readdir(DIR *dirp);
int closedir(DIR *dirp);
```

结构体*struct dirent*包含：

```c
struct dirent {
               ino_t          d_ino;       /* Inode number */
               off_t          d_off;       /* Not an offset; see below */
               unsigned short d_reclen;    /* Length of this record */
               unsigned char  d_type;      /* Type of file; not supported by all filesystem types */
               char           d_name[256]; /* Null-terminated filename */
           };
```

### 进程编程

#### ·进程的环境变量

环境变量可用environ来引用，以name = string的形式存取。

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
extern char** environ;
int main(int argc, char **argv)
{
    int i = 0;
    for(i = 0, environ[i] != NULL, i++)
    {
        printf("%s\n", environ[i]);
    }
    return 0;
}
```

> 二级指针的两种用途：
>
> 1、作为形参，用于传入一级指针并且需要修改一级指针的值的情况；
>
> 2、用于指向指针数组。

#### ·getenv函数和setenv函数

```c
#include <stdlib.h>//函数用于获取指定的环境变量
char *getenv(const char *name);//name为环境变量名称
```

```c
 #include <stdlib.h>
int setenv(const char *name, const char *value, int overwrite);//name环境变量名称，value环境变量值，overwrite为0直接返回，非0返回修改并返回新值
```

#### getpid()函数和getppid()函数

获取进程号，以及获取父进程号。

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main()
{
    pid_t pid1 = getpid();
    pid_t pid2 = getppid();
    printf("%u %u\n", pid1, pid2);
    getchar();
    return 1;
}
```

#### fork函数

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main()
{
    pid_t p = fork();
    pid_t pid1 = getpid();
    pid_t pid2 = getppid();
    printf("%u %u %u\n", p, pid1, pid2);
    return 1;
}
```

fork函数会复制出一个一模一样的子进程同步进行，原来进程的fork函数返回紫禁城的pid，子进程的fork函数返回0。

> 当不进行写操作时，子父进程共享数据，但当有写操作时，数据会分离。（写时复制）堆（malloc）和栈均不共享。
>
> 子父进程对数据作的任何修改，都不会影响另一方。

