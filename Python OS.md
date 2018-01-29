## Python笔记

### Python OS模块 常见操作示例 
#### os.path 模块中的路径名访问函数
- 分隔
```
    basename() 去掉目录路径, 返回文件名 
    dirname() 去掉文件名, 返回目录路径 
    join() 将分离的各部分组合成一个路径名 
    split() 返回 (dirname(), basename()) 元组 
    splitdrive() 返回 (drivename, pathname) 元组 
    splitext() 返回 (filename, extension) 元组 
 ```
- 信息
``` 
    getatime() 返回最近访问时间 
    getctime() 返回文件创建时间 
    getmtime() 返回最近文件修改时间 
    getsize() 返回文件大小(以字节为单位),如果name是目录返回0L 
    abspath() 返回绝对路径
    normpath() 返回路径的规范字符串形式
 ```
- 查询
``` 
    exists() 指定路径(文件或目录)是否存在 
    isabs() 指定路径是否为绝对路径 
    isdir() 指定路径是否存在且为一个目录 
    isfile() 指定路径是否存在且为一个文件 
    islink() 指定路径是否存在且为一个符号链接 
    ismount() 指定路径是否存在且为一个挂载点 
    samefile() 两个路径名是否指向同个文件
```      
     
#### os模块中的文件操作： 
- os 模块属性 
```
    linesep 用于在文件中分隔行的字符串 
    sep 用来分隔文件路径名的字符串 
    pathsep 用于分隔文件路径的字符串 
    curdir 当前工作目录的字符串名称 
    pardir (当前工作目录的)父目录字符串名称 
```
- os 模块操作
    - 重命名：os.rename(old, new)
    - 删除：os.remove(file)
    - 列出目录下的文件：os.listdir(path)
    - 获取当前工作目录：os.getcwd()
    - 改变工作目录：os.chdir(newdir)
    - 创建多级目录：os.makedirs(r"c:\python\test")
    - 创建单个目录：os.mkdir("test") 
    - 删除多个目录：os.removedirs(r"c:\python") #删除所给路径最后一个目录下所有空目录
    - 删除单个目录：os.rmdir("test")
    - 获取文件属性：os.stat(file)
    - 修改文件权限与时间戳：os.chmod(file) 
    - 执行操作系统命令：os.system("dir")
    - 启动新进程：os.exec(), os.execvp()
    - 在后台执行程序：osspawnv()
    - 终止当前进程：os.exit(), os._exit()
    - *******：os.ismount("c:\\") --> True
    - 搜索目录下的所有文件：os.path.walk()

- shutil模块对文件的操作
    - 复制单个文件：shultil.copy(oldfile, newfle)
    - 复制整个目录树：shultil.copytree(r".\setup", r".\backup") 
    - 删除整个目录树：shultil.rmtree(r".\backup")

- 临时文件的操作
    - 创建一个唯一的临时文件：tempfile.mktemp() --> filename 
    - 打开临时文件：tempfile.TemporaryFile()

- 内存文件（StringIO和cStringIO）操作 #cStringIO是StringIO模块的快速实现模块
    - 创建内存文件并写入初始数据：f = StringIO.StringIO("Hello world!") 
    - 读入内存文件数据：print f.read() #或print f.getvalue() --> Hello world!
    - 想内存文件写入数据：f.write("Good day!")
    - 关闭内存文件：f.close()