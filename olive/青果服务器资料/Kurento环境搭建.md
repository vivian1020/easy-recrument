环境：Ubuntu 14.04 LTS

1.安装add-apt-repository
	sudo apt-get install software-properties-common
 
2.安装kurento Media Server
	sudo add-apt-repository ppa:kurento/kurento
	sudo apt-get update
	sudo apt-get install kurento-media-server
	
3.启动kurento Media Server
	sudo service kurento-media-server start
	
4.安装JDK 1.7(必须安装1.7以上)
	下载JDK http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
	解压缩sudo tar zxvf ./jdk-7u45-linux-x64.tar.gz
	重命名sudo mv jdk1.7.0_45/ jdk1.7/
	设置环境变量sudo vi ~/.bashrc
		export JAVA_HOME=/opt/Java/jdk/jdk1.7
		export CLASSPATH=${JAVA_HOME}/lib
		export PATH=${JAVA_HOME}/bin:$PATH
	生效环境变量source ~/.bashrc
	
5.安装Maven 
	下载maven http://maven.apache.org/download.cgi 
	解压缩sudo tar -xzvf apache-maven-3.0.5-bin.tar.gz
	设置环境变量sudo vim /etc/profile
		export M2_HOME=/home/weibo/apache-maven-3.0.5
		export M2=$M2_HOME/bin
		export PATH=$M2:$PATH
	生效环境变量source profile
	
6.安装git
	sudo apt-get update
	sudo apt-get install git
	
7.安装Bower
	sudo apt-get install curl
	curl -sL https://deb.nodesource.com/setup | sudo bash -
	sudo apt-get install -y nodejs
	sudo npm install -g bower
	
8.下载DEMO代码
	git clone https://github.com/Kurento/kurento-tutorial-java.git
	
9.使用Maven编译启动DEMO
	cd kurento-tutorial-java/kurento-hello-world
	mvn clean compile exec:java
	
编译过程中遇到的问题及解决方式：
	 1.kurento-parent-pom版本问题：
		 The project org.kurento.tutorial:kurento-hello-world:[unknown-version]
		 Non-resolvable parent POM for org.kurento.tutorial:kurento-tutorial:6.4.1-SNAPSHOT: 
		 Could not find artifact org.kurento:kurento-parent-pom:pom:6.4.1-SNAPSHOT and 'parent.relativePath' points at wrong local POM @ org.kurento.tutorial:kurento-tutorial:6.4.1-SNAPSHOT
	 解决方式：
		发现在kurento POM文件中kurento-parent-pom定义的版本号为6.4.1-SNAPSHOT，查看kurento官网kurento-parent-pom并没有此版本，最新版本为6.4.0，将POM文件中kurento-parent-pom的Version改为6.4.0重新编译，问题解决；
	
	2. Failed to execute goal org.codehaus.mojo:exec-maven-plugin:1.3.2:exec (default) on project kurento-hello-world: Command execution failed. Process exited with an error: 1 (Exit value: 1) -> [Help 1]
org.apache.maven.lifecycle.LifecycleExecutionException: Failed to execute goal org.codehaus.mojo:exec-maven-plugin:1.3.2:exec (default) on project kurento-hello-world: Command execution failed. at Error (native)
	解决方式：
		在编译中需要创建文件夹，权限不足报出这个错；
		提升kurento文件夹权限：
		sudo chown username –R kurento-hello-world 
		sudo chmod +777 kurento-hello-world
		
	3.exec-maven-plugin:1.4.0:exec (default) @ kurento-hello-world ---bower                         
		EINVALID Failed to read /home/hzlinhai/kurento-tutorial-java/kurento-hello-world/bower.json
		Additional error details:
		Name must be lowercase, can contain digits, dots, dashes, "@" or spaces
		
	解决方式：
		bower包管理中name不支持-，将bower.json中的name改为kurento,重新编译；
		
10.启动DEMO
		https://localhost:8443