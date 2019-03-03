# architect
架构相关记录

# 一 试图用各种语言和框架搭建一个静态web-server，只处理get请求，对服务器端目录和文件列表展示.

1. python

  1.1 Python2  
    Windows: 
    下载python2.7安装包，安装后默认目录是c:\python27, 假设此值为{python-root},修改环境变量PATH使其包含{python-root}.
    
       > python -m pip install SimpleHTTPServer
       
	  然后到任意一个目录，执行 http-server  {port} ;比如:
	  > cd d:\my-python-projects\simpleServer
	  
	  > python -m SimpleHTTPServer  9099
	  
	Linux :Linux 自带python2.7,无需安装
	 $ pip install SimpleHTTPServer
	 
	 $ cd ~/my-python-projects\simpleServer
	 
	 $ python -m SimpleHTTPServer 9099
	 
	 到http://localhost:9099 查看结果

  1.2 Python3 
    下载python3.x安装包进行安装，修改环境变量PATH使其包含python3.x安装目录.
	将1.1步骤中的SimpleHTTPServer换成 http-server,按步骤执行即可。
	
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
   
  # 二 流媒体。
   1) ffserver搭建流媒体服务器
  支持HTTP推流，RTSP或HTTP播放。
  
   下载地址： https://ffbinaries.com/downloads ，选择ffmpeg 3.4 binaries ->ffserer ->选合适的CPU架构
   
   推流端下载ffmpeg.
   
   播放端建议选择VLC. 下载地址： https://www.videolan.org/vlc/download-windows.html
   
   ffserver在出现音频流后不能再出现单独的视频流,所以在配置文件中要注意。
   
   # fferver documentation:  
   https://ffmpeg.org/ffserver.html
   
   https://trac.ffmpeg.org/wiki/ffserver
   
   # ffmpeg docuemntation:
   http://ffmpeg.org/
   
   # ffserver 配置文件:
   
   HTTPPort 2000
   
RTSPPort 2001

MaxHTTPConnections 100

MaxClients 100

MaxBandwidth 1000

<Feed feed1.ffm>
        File feed1.ffm
        FileMaxSize 20M
        ACL allow 192.168.0.0  192.168.255.255
</Feed>

<Feed feed2.ffm>
        File feed2.ffm
        FileMaxSize 50M
        ACL allow 192.168.0.0  192.168.255.255
</Feed>

<Feed feed3.ffm>
        File feed3.ffm
        FileMaxSize 50M
        ACL allow 192.168.0.0  192.168.255.255
</Feed>

<Stream live.mp3>
        Feed feed1.ffm
	Format mp3
	
	AudioCodec libmp3lame
	AudioBitRate 64
	AudioChannels 2
	AudioSampleRate 44100
	AVOptionAudio flags +global_header
	NoVideo
</Stream>
 
<Stream live.sdp>
        Feed  feed2.ffm
	Format rtp
	
        AudioCodec  libmp3lame
	AudioBitRate 64
	AudioChannels 2
	AudioSampleRate 44100
	AVOptionAudio flags +global_header
	NoVideo
</Stream>

<Stream file.sdp>
        Feed  feed3.ffm
	Format rtp

        AudioCodec  libmp3lame
	AudioBitRate 64
	AudioChannels 2
	AudioSampleRate 44100
	AVOptionAudio flags +global_header
	NoVideo
</Stream>


<Stream stat.html>
	Format status
	ACL allow localhost
	ACL allow 192.168.0.0 192.168.255.255
</Stream>

<Redirect index.html>
	URL http://www.xyz.com/
</Redirect>

启动ffserver:  ./ffserver  -f  ffserver.conf ( ./ffserver -d -f  ffserver.conf  [以debug 模式启动])

windows上推流麦克风（只有音频无视频）：

 ffmpeg -f dshow -i audio="Microphone Array (Realtek High Definition Audio)" -vn -acodec libmp3lame -ac 1 -b:a 64k -ar 44100   http://192.168.0.150:1234/feed2.ffm 
 
 播放 ： vlc rtps://host:ip/live.sdp
 
 windows上推流文件(只有音频无视频)：
 
 ffmpeg -i missNoSay.mp3 -acodec  libmp3lame   http://192.168.0.150:1234/feed2.ffm
 
 推流flv文件(视频+音频）：
  ffmpeg -i out.flv   -acodec  libmp3lame   -vcodec libx264 -tune zerolatency -crf 18  http://192.168.0.150:1234/feed1.ffm
  
  
  #  容器(Container)
  
  ###1 Docker command:
  
  1. build docker image and run container

 docker build -t  <image-name>  .

 docker run -p exposedPort:innerPort  {image-name}

 docker tag <image-name>  username/repository:tag

 docker push username/repository:tag

 docker pull username/repository:tag

 docker run -p exposedPort:innerPort  username/repository:tag


enter an running container with console:

    docker run -it <continer> /bin/bash


2. run app/service and manage tasks

use swarm:

     docker swarm init

deploy an app:

      docker stack deploy -c docker-compose.yml  {appName}

list all app:

      docker stack ls 
  

list the services in an app:

      docker stack services {appName} 
	

list all services:

     docker service ls
  

list all tasks in a service: (a contianer instance in a service called as task )

    docker service ps  <serviceName>   ( serviceName=appName_service )
	

tear down an app:

     docker stack rm <appName>
	

3. docker-compose build
 

     docker-compose start
 
    docker-compose stop
 
    docker-compose up
 
    docker-compose down
 
4. docker network mgt

docker network ls

docker network rm xxx

docker netowrk prune

# 阿里云容器镜像加速器
https://8sdh8qa1.mirror.aliyuncs.com

。。。




 

  
  
 
 
   
   
   
    
   
   
   
   
   

