## 本地搭建一个服务器
#### 1. 本地Apache服务器
1. 首先在自己的电脑昵称的文件夹下建立一个sites文件夹，里面随便放一些后台假数据
2. 找到配置文件，给原来的文件备份
```
$cd /etc/apache2
$sudo cp httpd.conf httpd.conf.bak
///如果后续出现操作错误，需要还原时：$sudo cp httpd.conf.bak httpd.conf
```
*3. 修改配置文件
// 用vim编辑httpd.conf
$sudo vim httpd.conf
// 查找DocumentRoot  
/DocumentRoot
按下 i 进入编辑模式，可以看到有两个路径 把他们都改成你刚才建的那个Sites 文件夹的路径
再查找下 php，/php，定位到这一行后把光标移到最左边按下 x 删除“#”打开目录。（如果是10.10系统的话还有一步：查找Options 输入/Options 也可以目测自己找到图中的位置，在Options和Follow之间增加一个单词”Indexes“）Options Indexes FollowSymLinks Multiviews
改好之后先按下esc键退出编辑模式，再输入:wq 保存并退出 如果打错了不想保存就是 :q!
*4. 收尾工作与确认成功 
//拷贝配置文件
$sudo cp php.ini.default php.ini
// 重新启动apache服务器 之后下面说这句话是正常的
$sudo apachectl -k restart
再确认下到底成功了没有，就到浏览器里输入localhost如果能看到你放的文件的列表就表明配置成功了

*5. 注意事项
注意备份不要多次备份
注意vim编辑下全部使用英文符号和字母
服务器开关的命令就是:

```
$sudo apachectl -k start
$sudo apachectl -k stop
```

每次关机开机之后再想用服务器就要重新敲下开启的指令

#### 2. webDav服务器

接下来是WebDav服务器，这个是基于apache的，就是你apache已经启动了才能开启webDav服务器的。

当然如果apache已经完全配置好了那webDav也就很好配置了

WebDav完全可以当成一个网络共享的文件服务器使用！

1.继续修改

$ cd /etc/apache2

$ sudo vim httpd.conf

// 查找httpd-dav.conf

/httpd-dav.conf

还是和刚才一样按 i 编辑，定位到这一行后，光标移到最左边按 x 删除 # 号，

（如果你的电脑是10.10系统，还需要有以下下划线的操作：）

通过搜索找到这几行

LoadModule dav_module libexec/apache2/mod_dav.so

LoadModule dav_fs_module libexec/apache2/mod_dav_fs.so

LoadModule auth_digest_module libexec/apache2/mod_auth_digest.so

并且把他们行首的#号删除 （友情提示，他们这些行长的都很像一定要看清了别改错了）

 

按esc完成编辑，输入:wq退出

// 然后切换目录

$ cd /etc/apache2/extra

// 备份文件（切记只要备份一次就行）

$ sudo cp httpd-dav.conf httpd-dav.conf.bak

// 现在要编辑这个文件了

$ sudo vim httpd-dav.conf

// 查找Digest  把编辑模式从Digest改成Basic  还是那几步，改完了之后保存退出

/Digest

2.运行脚本文件

接下来要用到一个脚本文件下载地址在这

百度网盘的：http://pan.baidu.com/s/1jG7ogdS     密码是：yj9t

// 切换目录，可以使用鼠标把put脚本所在的文件夹直接拖到cd后面

$ cd 保存put脚本的目录

// 以管理员权限运行put配置脚本

$ sudo ./put

会先让你输入你电脑的密码，再给admin账号设置密码 如123456

设置完成后，他会显示一大串然后重启了apache服务器。

3.验证是否成功

到你的网络里看一下你现在连着网的ip地址

然后点开Finder --> 前往 -->连接服务器 -->里面输入 http://192.168.1.106/uploads （这个是举例，你要输入你自己的ip地址）

之后会弹出一个框，选择注册用户，账号admin，密码 如123456就能连接了

配置完成了之后就可以 在代码里发请求的url写上自己服务器内文件的url了。不连外网也可以执行下载上传操作。
