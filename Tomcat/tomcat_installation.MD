# Tomcat installation on EC2 instance
To install java 11 on your EC2 instance follow the slide from the class.
### Pre-requisites
1. EC2 instance with Java 11
### Install Apache Tomcat
1. Download tomcat packages from  https://tomcat.apache.org/download-80.cgi onto /opt on EC2 instance
   > Note: Make sure you change `<version>` with the tomcat version which you download. 
   ```sh 
   # Create tomcat directory
   cd /opt
   mkdir tomcat
   cd tomcat
   wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.78/bin/apache-tomcat-8.5.78.tar.gz
   tar -xvzf /opt/apache-tomcat-<version>.tar.gz
   ```
1. give executing permissions to startup.sh and shutdown.sh which are under bin. 
   ```sh
   chmod +x /opt/tomcat/apache-tomcat-<version>/bin/startup.sh 
   chmod +x /opt/tomcat/apache-tomcat-<version>/bin/shutdown.sh
   ```
   > Note: you may get below error while starting tomcat incase if you dont install Java   
   `Neither the JAVA_HOME nor the JRE_HOME environment variable is defined At least one of these environment variable is needed to run this program`
1. create link files for tomcat startup.sh and shutdown.sh 
   ```sh
   ln -s /opt/tomcat/apache-tomcat-<version>/bin/startup.sh /usr/local/bin/tomcatup
   ln -s /opt/tomcat/apache-tomcat-<version>/bin/shutdown.sh /usr/local/bin/tomcatdown
   tomcatup
   ```
  #### Check point :
access tomcat application from browser on port 8080  
 - http://<Public_IP>:8080

#### Check point :
Access tomcat application from browser on port 8080  
 - http://<Public_IP>:8080

1. now application is accessible on port 8080. But tomcat application doesnt allow to login from browser. changing a default parameter in context.xml does address this issue
   ```sh
   #search for the file "context.xml" with the command below
   find /opt -name context.xml
   ```
1. above command gives 4 context.xml files. 
2. vim into the files and comment `< Value ClassName...../>` field on files which are under the following directory directory. 
3. ~/host-manager/........
4. ~/manager/........
5. To comment out use <!-- value ClassName....... -->
After that restart tomcat services to effect these changes. 
At the time of writing this lecture below 2 files are updated. 
   ```sh 
   /opt/tomcat/webapps/host-manager/META-INF/context.xml
   /opt/tomcat/webapps/manager/META-INF/context.xml
   
   # Restart tomcat services
   tomcatdown  
   tomcatup
   ```
1. Update users information in the tomcat-users.xml file
2. Go into your tomcat home directory
3. cd /opt/tomcat/apache-tomcat-<version>
And Add below users to conf/tomcat-users.xml file
   ```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="deployer" password="deployer" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>
   ```
1. Restart serivce and try to login to tomcat application from the browser. This time it should be Successful

