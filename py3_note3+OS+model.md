
## OS

* os.access(path,mode)主要是测试
文件的是否存在(调用os.F_OK属性)  
文件有没有可读性(调用os.R_OK)  
文件能不能写入(调用os.W_OK)  
文件能不能执行(调用os.X_OK)方法返回的是bool值 


```python
import os, sys
#相对引用为位置需要引入.
# 假定 /tmp/foo.txt 文件存在，并有读写权限

ret = os.access("./tmp/foo.txt", os.F_OK)
print ("F_OK - 返回值 %s"% ret)

ret = os.access("./tmp/foo.txt", os.R_OK)
print ("R_OK - 返回值 %s"% ret)

ret = os.access("./tmp/foo.txt", os.W_OK)
print ("W_OK - 返回值 %s"% ret)

ret = os.access("./tmp/foo.txt", os.X_OK)
print ("X_OK - 返回值 %s"% ret)
```

    F_OK - 返回值 True
    R_OK - 返回值 True
    W_OK - 返回值 False
    X_OK - 返回值 True
    

* os.chdir() 方法用于改变当前工作目录到指定的路径。
* os.chdir(path) path -- 要切换到的新路径。如果允许访问返回 True , 否则返回False。
* os.fchdir(fd); 通过文件描述符改变当前工作目录   Unix, Windows 上可用
* os.getcwd() 返回当前目录
* os.fstat(fd) 方法用于返回文件描述符fd的状态，类似 stat()
* os.getcwdu() 方法用于返回一个当前工作目录的Unicode对象。  Unix, Windows 上可用



```python
#工作目录更改
import os, sys

path ='G:\Jupyter\Learning'

retval = os.getcwd()
print ("当前工作目录为 %s" % retval)

# 修改当前工作目录
os.chdir( path )

# 查看修改后的工作目录
retval = os.getcwd()

print ("目录修改成功 %s" % retval)

#import os, sys

# 首先到目录 "/var/www/html" 
#os.chdir("/var/www/html" )

# 输出当前目录
#print ("当前工作目录为 : %s" % os.getcwd())

# 打开新目录 "/tmp"
#fd = os.open( "/tmp", os.O_RDONLY )

# 使用 os.fchdir() 方法修改到新目录
#os.fchdir(fd)

# 输出当前目录
#print ("当前工作目录为 : %s" % os.getcwd())

# 关闭打开的目录
#os.close( fd )
```

    当前工作目录为 G:\Jupyter\Learning
    目录修改成功 G:\Jupyter\Learning
    


```python
#import os, sys
# 切换到 "/var/www/html" 目录
#os.chdir("/var/www/html" )

# 打印当前目录
#print ("当前工作目录 : %s" % os.getcwdu())

# 打开 "/tmp"
#fd = os.open( "/tmp", os.O_RDONLY )

# 使用 os.fchdir() 方法修改目录
os.fchdir(fd)

# 打印当前目录
print ("当前工作目录 : %s" % os.getcwdu())

# 关闭文件
os.close( fd )
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-3-6a039090aaab> in <module>()
         10 
         11 # 使用 os.fchdir() 方法修改目录
    ---> 12 os.fchdir(fd)
         13 
         14 # 打印当前目录
    

    AttributeError: module 'os' has no attribute 'fchdir'


* os.chflags() 方法用于设置路径的标记为数字标记。多个标记可以使用 OR 来组合起来。只支持在 Unix 下使用。

* os.chmod(path, mode) 方法用于更改文件或目录的权限。无返回值
* path -- 文件名路径或目录路径。
* flags -- 可用以下选项按位或操作生成， 目录的读权限表示可以获取目录里文件名列表， ，执行权限表示可以把工作目录切换到此目录 ，删除添加目录里的文件必须同时有写和执行权限 ，文件权限以用户id->组id->其它顺序检验,最先匹配的允许或禁止权限被应用。
    * stat.S_IXOTH: 其他用户有执行权0o001
    * stat.S_IWOTH: 其他用户有写权限0o002
    * stat.S_IROTH: 其他用户有读权限0o004
    * stat.S_IRWXO: 其他用户有全部权限(权限掩码)0o007
    * stat.S_IXGRP: 组用户有执行权限0o010
    * stat.S_IWGRP: 组用户有写权限0o020
    * stat.S_IRGRP: 组用户有读权限0o040
    * stat.S_IRWXG: 组用户有全部权限(权限掩码)0o070
    * stat.S_IXUSR: 拥有者具有执行权限0o100
    * stat.S_IWUSR: 拥有者具有写权限0o200
    * stat.S_IRUSR: 拥有者具有读权限0o400
    * stat.S_IRWXU: 拥有者有全部权限(权限掩码)0o700
    * stat.S_ISVTX: 目录里文件目录只有拥有者才可删除更改0o1000
    * stat.S_ISGID: 执行此文件其进程有效组为文件所在组0o2000
    * stat.S_ISUID: 执行此文件其进程有效用户为文件所有者0o4000
    * stat.S_IREAD: windows下设为只读
    * stat.S_IWRITE: windows下取消只读


```python
import os, sys, stat

# 假定 /tmp/foo.txt 文件存在，设置文件可以通过用户组执行

os.chmod("./tmp/foo.txt", stat.S_IXGRP)

# 设置文件可以被其他用户写入
os.chmod("./tmp/foo.txt", stat.S_IWOTH)

print ("修改成功!!")
```

    修改成功!!
    

* os.chown() 方法用于更改文件所有者，如果不修改可以设置为 -1, 你需要超级用户权限来执行权限修改操作。
只支持在 Unix 下使用。

* os.chroot(path) 方法用于更改当前进程的根目录为指定的目录，使用该函数需要管理员权限。

* os.close(fd) 方法用于关闭指定的文件描述符 fd。
* os.closerange(fd_low, fd_high);用于关闭所有文件描述符 fd，从 fd_low (包含) 到 fd_high (不包含), 错误会忽略


```python
import os, sys

# 打开文件
fd = os.open( "foo.txt", os.O_RDWR|os.O_CREAT )

#  写入字符串
os.write(fd, "This is test".encode())#str bytes
#如果里面有内容会覆盖的
# 关闭文件
ret=os.read(fd,10)
print (ret)
os.close( fd )
print ("关闭文件成功!!")
```

    b'\xb8\xf6\xb7\xc7\xb3\xa3\xba\xc3\xb5\xc4'
    关闭文件成功!!
    


```python
import os, sys

# 打开文件
fd = os.open( "foo.txt", os.O_RDWR|os.O_CREAT )

# 写入字符串
os.write(fd, "This is test!".encode())

# 关闭文件
os.closerange( fd, fd)

print ("关闭文件成功!!")
```

    关闭文件成功!!
    

* os.write() 方法用于写入字符串到文件描述符 fd 中. 返回实际写入的字符串长度。

* os.dup(fd) 用于复制文件描述符 fd。
* os.dup2(fd) Unix, Windows 上可用。作用一样

* os.link(src, dst) 用于创建硬链接，名为参数 dst，指向参数 src 只支持在 Unix, Windows 下使用
* os.symlink(src, dst) 用于创建一个软链接

* os.listdir(path)用于返回指定的文件夹包含的文件或文件夹的名字的列表。这个列表以字母顺序。只支持在 Unix, Windows 下使用

* os.read(fd,n) 用于从文件描述符 fd 中读取最多 n 个字节，返回包含读取字节的字符串，文件描述符 fd对应文件已达到结尾, 返回一个空字符串。在Unix，Windows中有效
* os.remove(path) 用于删除指定路径的文件。如果指定的路径是一个目录，将抛出OSError。在Unix，Windows中有效

* os.unlink(path) 方法用于删除文件,如果文件是一个目录则返回一个错误。

* os.removedirs(path) 方法用于递归删除**目录**。像rmdir(), 如果子文件夹成功删除, removedirs(path)才尝试它们的父文件夹,直到抛出一个error(它基本上被忽略,因为它一般意味着你文件夹不为空)。

* os.rename(src, dst) os.rename() 方法用于命名文件或目录，从 src(要修改的) 到 dst（修改后的）,如果dst是一个存在的目录, 将抛出OSError。
* os.renames(old, new) os.renames() 方法用于递归重命名目录或文件。类似rename()。new --文件或目录的新名字。甚至可以是包含在目录中的文件，或者完整的目录树。

* os.rmdir() 方法用于删除指定路径的目录。仅当这文件夹是空的才可以, 否则, 抛出OSError。

* os.tempnam(dir, prefix) 方法用于返回唯一的路径名用于创建临时文件。prefix -- 临时文件前缀dir -- 要创建的临时文件路径

* os.utime(path, times) 用于设置指定路径文件最后的修改和访问时间,在Unix，Windows中有效。



```python
#设置指定路径文件最后的修改和访问时间
import os, sys

# 显示文件的 stat 信息
stinfo = os.stat('test.txt')
print (stinfo)

# 使用 os.stat 来接收文件的访问和修改时间
print ("test.txt 的访问时间: %s" %stinfo.st_atime)
print ("test.txt 的修改时间: %s" %stinfo.st_mtime)

# 修改访问和修改时间
os.utime("test.txt",(1330712280, 1330712292))
print ("done!!")
```

    os.stat_result(st_mode=33206, st_ino=7599824371190512, st_dev=3794061894, st_nlink=1, st_uid=0, st_gid=0, st_size=78, st_atime=1330712280, st_mtime=1514165222, st_ctime=1508578846)
    test.txt 的访问时间: 1330712280.0
    test.txt 的修改时间: 1514165222.9670544
    done!!
    


```python
#import os, sys

# 前缀为 runoob 的文件
#tmpfn = os.tempnam('/tmp/runoob,'runoob')

#print ("这是一个唯一路径:")
#print (tmpfn)
```


```python
#import os, sys

# 打开文件
#path = "/var/www/html/foo.txt"
#fd = os.open( path, os.O_RDWR|os.O_CREAT )

# 关闭文件
#os.close( fd )

# 创建以上文件的拷贝
#dst = "/tmp/foo.txt"
#os.link( path, dst)

#print ("创建硬链接成功!!")
```


```python
#顺序返回目录下的文件名和文件夹名
import os, sys
dirs=os.listdir(os.getcwd())
for i in dirs:
    print (i,end='  ')
```

    .ipynb_checkpoints  data.pkl  foo.txt  fseek.txt  photo  py3_note1 common .ipynb  py3_note2+structure+IO+File.ipynb  py3_note2+structure+IO+File.md  py3_note3 OS model.ipynb  py3_note4 error OOP.ipynb  py3_note5 Standard library.ipynb  py3_note6 example.ipynb  runoob.txt  test.ipynb  test.txt  tmp  Untitled.ipynb  Untitled1.ipynb  技巧.txt  


```python
import os, sys

# 创建的目录
path = "./tmp/hourly"

os.mkfifo( path, 0644 )

print ("路径被创建")
```


      File "<ipython-input-11-e650ad152190>", line 6
        os.mkfifo( path, 0644 )
                            ^
    SyntaxError: invalid token
    

