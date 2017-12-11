### 1.下载jenkins docker image
在hub.docker.com（https://hub.docker.com/r/jenkinsci/jenkins/）上可以看到jenkins的image的下载方法。
在命令行执行如下命令将下载jenkins的image：
sudo docker pull jenkinsci/jenkins
 
### 2.运行jenkins docker
创建jenkins_home
<pre><code> cd /home/USER_NAME</code><br>
<code> mkdir jenkins_home_docker</code></pre> 

准备jenkins的log配置文件：
<pre><code>cd /home/<USER_NAME>/jenkins_home_docker</code><br></pre>
<pre><code>cat > log.properties <<EOF <br>
handlers=java.util.logging.ConsoleHandler<br>
jenkins.level=FINEST<br>
java.util.logging.ConsoleHandler.level=FINEST<br>
EOF</code></pre>
 
运行jenkins的docker image：
<pre><code>sudo docker run --name myjenkins -p 8088:8080 -p 50000:50000 -d --env JAVA_OPTS="-Xmx8192m" --env JAVA_OPTS="-Djava.util.logging.config.file=/home/USER_NAME/jenkins_home_docker/log.properties"  --env JENKINS_SLAVE_AGENT_PORT=50000 -v /home/USER_NAME/jenkins_home_docker:/var/jenkins_home  jenkinsci/jenkins</code></pre>
 
### 3。docker命令行解释：
docker实例的名字： <code>--name myjenkins</code>，此docker实例的名字为myjenkins。
docker端口映射： <code>-p IP:host_port:container_port</code>， -p 8088:8080 将docker里的8080映射到host中的8088。
环境变量： <code>--env name=value</code>。
目录映射： <code>-v localdir:dockerdir</code>, -v /home/USER_NAME/jenkins_home_docker:/var/jenkins_home  jenkinsci/jenkins将docker里的JENKINS_HOME /var/jenkins_home映射为host中的/home/osboxes/jenkins_home_docker。
运行的docker image： jenkinsci/jenkins
-d: docker instance将作为demon在后台运行。
如果是java1.7及以前版本，组好设定--env  JAVA_OPTS=”-Xmx8192m -XX:PermSize=256m -XX:MaxPermSize=1024m”, java1.8后的直接--env JAVA_OPTS="-Xmx8192m"。
 
检查jenkins docker是否运行：
<pre><code>sudo docker ps</code></pre>
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
14fc572bc91c jenkinsci/jenkins "/bin/tini -- /usr/lo" 18 hours ago Up 18 hours 0.0.0.0:50000->50000/tcp, 0.0.0.0:8088->8080/tcp myjenkins
