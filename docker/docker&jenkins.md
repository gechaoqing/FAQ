2. ����jenkins docker image
��hub.docker.com��https://hub.docker.com/r/jenkinsci/jenkins/���Ͽ��Կ���jenkins��image�����ط�����
��������ִ�������������jenkins��image��
sudo docker pull jenkinsci/jenkins
 
3. ����jenkins docker
������֮ǰ�����Ȳ鿴jenkins��dockerfile��https://github.com/jenkinsci/docker/blob/master/Dockerfile����ͨ��jenkins��dockerfile��ſ����˽�jenkins��docker image�������������ʲô��
 
׼��jenkins��log�����ļ���
cd /home/osboxes/jenkins_home_docker
cat > log.properties <<EOF
handlers=java.util.logging.ConsoleHandler
jenkins.level=FINEST
java.util.logging.ConsoleHandler.level=FINEST
EOF
 
����jenkins��docker image��
sudo docker run --name myjenkins -p 8088:8080 -p 50000:50000 -d --env JAVA_OPTS="-Xmx8192m" --env JAVA_OPTS="-Djava.util.logging.config.file=/home/osboxes/jenkins_home_docker/log.properties"  --env JENKINS_SLAVE_AGENT_PORT=50000 -v /home/osboxes/jenkins_home_docker:/var/jenkins_home  jenkinsci/jenkins
 
docker�����н��ͣ�
dockerʵ�������֣� --name myjenkins����dockerʵ��������Ϊmyjenkins��
docker�˿�ӳ�䣺 -p IP:host_port:container_port�� -p 8088:8080 ��docker���8080ӳ�䵽host�е�8088��
���������� --env name=value��
Ŀ¼ӳ�䣺 -v localdir:dockerdir, -v /home/osboxes/jenkins_home_docker:/var/jenkins_home  jenkinsci/jenkins��docker���JENKINS_HOME /var/jenkins_homeӳ��Ϊhost�е�/home/osboxes/jenkins_home_docker��
���е�docker image�� jenkinsci/jenkins
-d: docker instance����Ϊdemon�ں�̨���С�
�����java1.7����ǰ�汾������趨--env  JAVA_OPTS=��-Xmx8192m -XX:PermSize=256m -XX:MaxPermSize=1024m��, java1.8���ֱ��--env JAVA_OPTS="-Xmx8192m"��
 
���jenkins docker�Ƿ����У�
sudo docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
14fc572bc91c jenkinsci/jenkins "/bin/tini -- /usr/lo" 18 hours ago Up 18 hours 0.0.0.0:50000->50000/tcp, 0.0.0.0:8088->8080/tcp myjenkins