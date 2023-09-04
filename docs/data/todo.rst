WebDAV 协议访问
---------------

由于不是所有实例都提供SSH端口的访问方式，对于需要批量上传或下载的用户，ChinaSRC还提供了WebDAV协议的文件访问方式。

WebDAV是一种通信协议，支持大批量的文件传输。对于用户来说，相当于将ChinaSRC的服务器以网盘的形式挂载到用户的个人电脑，用户将个人电脑里的数据拷贝或者上传到ChinaSRC的服务器上。

WebDAV协议访问的地址是\ |webdav url| \ 。用户验证请使用ChinaSRC用户名和密码。

Windows系统建议使用\ `RaiDrive <https://www.raidrive.com/>`__\ 或\ `Cyberduck <https://cyberduck.io/>`__\ 。Mac系统建议使用\ `Cyberduck <https://cyberduck.io/>`__\ 。Linux系统建议使用\ `rclone <https://rclone.org/docs/>`__\ 。

为了方便，ChinaSRC提供了上述软件，请根据需要下载：

**Mac：**

- `Cyberduck for Mac <http://xx.xx.xx.xx:8080/help/files/Cyberduck-7.5.1.33324-mac.zip>`_ 

**Windows：**

- `RaiDrive <http://xx.xx.xx.xx:8080/help/files/RaiDrive_2020.6.36.exe>`_
- `Cyberduck for Win <http://xx.xx.xx.xx:8080/help/files/Cyberduck-Installer-7.5.1.33324-win.exe>`_ 
- `WinSCP <http://xx.xx.xx.xx:8080/help/files/WinSCP-5.17.10-Setup.exe>`_

**Linux：**

- `rclone <https://downloads.rclone.org/v1.55.1/rclone-v1.55.1-linux-amd64.zip>`_

Cyberduck使用说明
~~~~~~~~~~~~~~~~~~~~~~~

打开Cyberduck，点击“新建连接”，按照下图所示填写连接方式，要选择“WebDAV(HTTPS)”方式。用户验证请使用ChinaSRC用户名和密码。

|cyberduck connect|

连接过程中出现任何提示，直接点“继续”。连接成功后，可以使用软件的创建文件夹、上传等功能。注意，使用共享实例，比如交互式的JupyterLab、RStudio、Stata、MATLAB的用户，目标文件夹是“MyData”文件夹。软件的“操作”按钮下有“新建文件夹”、“上传”等功能。

RaiDrive使用说明
~~~~~~~~~~~~~~~~~~~~~~

下载\ `RaiDrive <http://xx.xx.xx.xx:8080/help/files/RaiDrive_2020.6.36.exe>`_\ 并安装后，点击窗口顶部的“添加”按钮，按照下图所示创建WebDAV驱动器。

|raidrive connect|

点击确定后，会跳出一个Windows资源浏览器窗口。RaiDrive已经把新添加的WebDAV连接创建成了一个网络存储驱动器，可以像操作本地磁盘一样从其他驱动器里拖拽文件或者拷贝到这个驱动器下的子目录中。

.. attention:: 

   RaiDrive WebDAV驱动器功能有局限

   - WebDAV驱动器里的文件不支持编辑，只支持创建和删除。
   - 根目录下不能创建目录或文件，只能在列出的顶层目录下操作。
   
WinSCP使用说明
~~~~~~~~~~~~~~~~~~~~~~

打开 WinSCP ，在登录对话框中“新建站点”。按下图所示，选择“WebDAV”-“TLS/SSL隐式加密”、填入主机名和端口\ |webdav url| \ ，使用ChinaSRC用户名和密码进行登录

|winscp connect|

rclone使用说明
~~~~~~~~~~~~~~~~~~~~~~~
1.下载rclone压缩包

官方链接地址 https://downloads.rclone.org/v1.55.1/rclone-v1.55.1-linux-amd64.zip


2.解压，并将rclone程序拷贝到 $PATH 中，给予相应权限

.. code:: bash

   unzip rclone-v1.55.1-linux-amd64.zip
   cd rclone-v1.55.1-linux-amd64
   cp rclone /usr/bin/
   chown root:root /usr/bin/rclone
   chmod 755 /usr/bin/rclone


3.配置 rclone config

.. code:: bash

   #rclone config
   cd rclone-v1.55.1-linux-amd64
   > n    #新建连接
   name> remote    #设置连接名称
   Storage> webdav    #设置存储类型
   url> https://webdav_ip:4918   #设置webdav服务端地址
   vendor>other     #设置服务端vendor
   user> username      #设置ChinaSRC用户名
   y/g/n> y
   password: ******    #设置ChinaSRC密码
   bearer_token>       #键入回车，跳过
   Edit advanced config? (y/n)  #键入回车，跳过
   Remote config       #键入回车，跳过
   e/n/d/r/c/s/q> q     #配置完成，退出

以上操作结束后，可以在/root/.config/rclone/rclone.conf 中看到相应配置文件


4.关闭证书检查

.. attention:: 
   注意：此项必须关闭，不然远程操作会报错。

有两种关闭检查的方式，如下：

- 1.在执行命令时带上\ ``--no-check-certificate``\ 参数，如：``rclone ls remote: --no-check-certificate``
- 2.在环境变量里指定，如：``export RCLONE_NO_CHECK_CERTIFICATE=true``


5.rclone常用操作

.. code:: bash

   # 列出远程目录，remote替换为配置时设置的连接名称
   rclone lsd remote：
   
   # 将本地文件复制到远程的MyData目录
   rclone copy -P /tmp/*  remote:/MyData/

   #远程的文件复制到本地
   rclone copy -P remote:/MyData/rclone-v1.55.1-linux-amd64.zip /tmp


6.其它操作可参考官方文档：https://rclone.org/docs/

虚拟机实例内访问Home目录
------------------------

ChinaSRC内的虚拟机实例访问共享文件系统上的Home目录的机制类似于用户从外部访问WebDAV服务。目前ChinaSRC提供Windows和Linux虚拟机，连接方法分别如下:

1. Linux虚拟机：在Linux虚拟机镜像中已经预先做好了WebDAV卷的自动挂载，挂载点是/webdav，可以直接访问 /webdav/MyData/ 目录下的文件。

2. Windows虚拟机：在Windows虚拟机中也预先做好了WebDAV卷的自动挂载，原理是使用Windows自带的WebDAV Client工具挂载了WebDAV服务端提供的网络存储。

其中，\ ``MyData``\ 存放的是个人数据，\ ``ProjectGroup(public_cluster)``\ 存放的是\ ``public_cluster``\ 项目组中的数据，其它项目组同理。如下图:

|windows webdav client|

算例数据可以通过这种方式拷贝到C盘目录去处理，处理完再从C盘拷贝到网络存储的目录中，然后从实例的数据“管理页面”下载到自己的电脑。

.. hint:: 

   通过系统ChinaSRC的数据管理页面下载时，对下载的文件数量有限制，建议先在虚拟机中对处理结果打包后再传输。

Windows系统自带的WebDAV Client可以满足大多数文件拷贝需求，但是对单个文件的大小限制为4G，文件如果大于4G会报错。

|transfer error|

此时，需要在虚拟机里安装WinSCP工具来拷贝文件，参考前面软件下载部分。以下分别以RaiDrive软件和WinSCP为例：

RaiDrive
~~~~~~~~~~~~~

首先下载RaiDrive并上传到虚拟机中，安装RaiDrive需要\ ``.net framework``\ 的支持，会提示下载安装，默认安装即可。另外，可能会要求重启虚拟机系统，重启即可。

安装完成后，新建站点并输入以下信息进行连接，“文件协议”-“WebDAV”，不加密，连接地址为\ ``http://10.0.255.254:4918``\ ，Account即ChinaSRC的用户名、密码。

|winscp vm connect|

|winscp vm connect 2|

连接建立成功后，会自动打开数据目录，再进行文件传输即可。

|winscp vm connect 3|

WinSCP
~~~~~~~~~~~~

在虚拟机内安装WinSCP，安装后重启虚拟机系统。 按如下新建会话后点击登录。

|winscp vm connect 4|

连接建立成功后，会自动打开数据目录，再进行文件传输即可。

|winscp vm connect 5|
