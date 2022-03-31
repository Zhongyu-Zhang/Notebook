- [ ] **linux系统下解压zip压缩包乱码问题：** 

  `unzip -O CP936 xxx.zip`  

  可能需在环境变量中，指定unzip参数，总是以指定的字符集显示和解压文件，/etc/environment中加入2行
  `UNZIP="-O CP936"`

  `ZIPINFO="-O CP936"`

- [ ] **linux系统下安装deb格式安装包：**

  `1.cd 到安装包的目录`
  `2.sudo dpkg -i file.deb（如果是red hat linux的rpm格式，用rpm -ivh file.rpm） `

- [ ] **linux系统调试环境变量：**
  `vi ~/.bashrc`
  根据需求增加或删除环境变量
  `PYTHON_HOME=/usr/*****(eg:python-3.6.5)`
  `PATH=$PATH:PYTHON_HOME/bin`
  最后需要运行一下，才可生效
  `source ~/.bashrc`

- [ ] **linux系统安装软件时依赖软件包未安装：**
  用指令`sudo dpkg -i ***.deb`安装软件包时提示依赖软件包未安装时，使用以下命令自动修复：
  `sudo apt-get install -f`

- [ ] **linux系统中docker的运行方法：**

  `docker run -it -v ~/Desktop/umbrella/vxstain:/latex fredqi/texlive /bin/bash`

  查看docker的所有容器：`docker ps -a`

  删除docker的某个容器：`docker rm -f 编号`

  查看docker的所有镜像：`docker image ls `

  删除docker的某个镜像：`docker rmi -f 编号`

  进入一个已有的容器：`docker attach 编号`


