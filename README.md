# architect
架构相关记录

一 试图用各种语言和框架搭建一个静态web-server，只处理get请求，对服务器端目录和文件陈列.

1. python

1.1 Python2  
   Windows: 
   下载python2.7安装包，安装后默认目录是c:\python27, 假设此值为{python-root},修改环境变量PATH使其包含{python-root}.
      > python -m pip install http-server
	  然后到任意一个目录，执行 http-server  {port} ;比如:
	  > cd d:\my-python-projects\simpleServer
	  > python http-server  9099
	  
	Linux :Linux 自带python2.7,无需安装
	 $ pip install http-server
	 $ cd ~/my-python-projects\simpleServer
	 $ python http-server 9099
	 
	 到http://localhost:9099 查看结果

1.2 Python3 
    下载python3.x安装包进行安装，修改环境变量PATH使其包含python3.x安装目录.
	将1.1步骤中的http-server换成SimpleHTTPServer ,按步骤执行即可。
	
	python 网站：https://www.python.org/
	python 下载: https://www.python.org/downloads/
	
2. nodeJS
   Linux 安装nodeJS:
   $ yum install nodejs -y
   $ yum update openssl -y
   $ npm install -g http-server
   Window 安装nodeJS:
    到nodejs网站下载稳定版安装包进行安装.
   
   安装完成后，输入node -v 和npm -v 检验是否安装成功。(如果是centos7.2或者Redhat 7.x , 则分别是v6.12.3 , v3.10.10)
 
   到你的目录下执行:
   http-server 
   到http://localhost:8080 查看结果.
   如果当前目录有html,css,javascripts则就是个静态web-server
   
   nodeJS 网站: https://nodejs.org/en/
   nodeJS 下载: https://nodejs.org/en/download/
   
   
   
   
   

