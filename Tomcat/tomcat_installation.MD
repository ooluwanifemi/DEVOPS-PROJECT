# Tomcat installation on EC2 instance

To install Java 11 on your EC2 instance, follow the slides from the class.

## Pre-requisites

- EC2 instance with Java 11

## Installation Steps

1. **Launch an EC2 Instance**

    - Launch an EC2 instance on AWS with the necessary instance type and configuration.
    - Make sure all ports are open (All traffic) to allow access to the Tomcat server.

2. **Update the Server**

    - SSH into the EC2 instance using the appropriate credentials.
    - Run the following commands to update the server:

        ```
        sudo apt update
        sudo apt upgrade
        ```

3. **Install Java 11**

    - Install Java 11 on the EC2 instance by running the following command:

        ```
        sudo apt install openjdk-11-jdk
        ```

4. **Install Apache Tomcat**

    - Change the directory to /opt:

        ```
        cd /opt
        ```

    - Create a tomcat directory and navigate into it:

        ```
        sudo mkdir tomcat
        cd tomcat
        ```

    - Download the Tomcat package using wget. Replace `<version>` with the Tomcat version you want to install:

        ```
        sudo wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.75/bin/apache-tomcat-9.0.75.tar.gz
        ```

    - Extract the downloaded tar.gz file:

        ```
        sudo tar -xvzf apache-tomcat-9.0.75.tar.gz
        ```

    - Remove the .tar.gz file:

        ```
        sudo rm apache-tomcat-9.0.75.tar.gz
        ```

    - Navigate into the apache-tomcat-9.0.75 folder:

        ```
        cd apache-tomcat-9.0.75
        ```

    - Go into the bin folder:

        ```
        cd bin
        ```

    - Provide executing permissions to startup.sh and shutdown.sh:

        ```
        sudo chmod +x startup.sh
        sudo chmod +x shutdown.sh
        ```

5. **Create Links for Tomcat Startup and Shutdown**

    - Create symbolic links for startup.sh and shutdown.sh to make them easily accessible:

        ```
        sudo ln -s /opt/tomcat/apache-tomcat-9.0.75/bin/startup.sh /usr/local/bin/tomcatup
        sudo ln -s /opt/tomcat/apache-tomcat-9.0.75/bin/shutdown.sh /usr/local/bin/tomcatdown
        ```

    - Start Tomcat by running the following command:

        ```
        tomcatup
        ```

6. **Update Context.xml File**

    - Search for the context.xml file using the following command:

        ```
        sudo find /opt -name context.xml
        ```

    - Open each context.xml file found and comment out the `<Value ClassName.../>` field in the following directories:

        ```
        ~/apache-tomcat-9.0.75/webapps/host-manager/META-INF/context.xml
        ~/apache-tomcat-9.0.75/webapps/manager/META-INF/context.xml
        ```

    - Restart Tomcat services to apply the changes:

        ```
        tomcatdown
        tomcatup
        ```

7. **Update Users in tomcat-users.xml**

    - Go to the Tomcat home directory:

        ```
        cd /opt/tomcat/apache-tomcat-9.0.75
        ```

    - Edit the conf/tomcat-users.xml file and add the following users:

        ```xml
        <role rolename="manager-gui"/>
        <role rolename="manager-script"/>
        <role rolename="manager-jmx"/>
        <role rolename="manager-status"/>
        <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
        <user username="deployer" password="deployer" roles="manager-script"/>
        <user username="tomcat" password="s3cret" roles="manager-gui"/>
        ```

8. **Restart Tomcat Service**

    - Restart the Tomcat service to apply the changes:

        ```
        tomcatdown
        tomcatup
        ```

9. **Verify Access to Tomcat Application**

    - Open a web browser and access the Tomcat application from the EC2 instance's public IP address on port 8080:

        ```
        http://<Public_IP>:8080
        ```

    - You should now be able to access the Tomcat application.

