### 编写new_tomcat脚本(即DockerFile) ###

```shel
From   centos  #说明镜像源
MAINTAINER Psq<1636538091@qq.com>  #设置作者和其邮箱
#宿主机当前上下文的c.txt拷贝到容器/usr/local路径下,并且重命名为cincontainer.txt
COPY c.txt /usr/local/cincontainer.txt
#把java与tomcat添加到容器中,并且解压-->这是ADD与COPY的重要区别
ADD jdk-8u171-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.8.tar.gz /usr/local/
#安装vim编辑器
RUN yum -y install vim
#设置工作访问时候的WORKDIR路径，即进入容器时的目录
ENV MYPATH /usr/local
WORKDIR $MYPATH
#配置java 和 tomcat环境变量
ENV JAVA_HOME /usr/local/jdk1.8.0_171
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.8
ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.8
ENV PATH $PAHT:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
#暴露容器运行时的端口（即监听端口）
EXPOSE 8080
#ENTRYPOINT 可以在run命令后追加命令，而使用CMD时docker run后面的命令会覆盖前面的命令，即只有最后一条CMD 命令有执行效果，所以需要灵活运用这两个命令来指定容器运行时要执行的命令
# ENTRYPOINT ["/usr/local/apache-tomcat-9.0.9/bin/startup.sh"]
# CMD ["/usr/local/apache-tomcat-9.0.8/bin/catalina.sh","run"]
CMD /usr/local/apache-tomcat-9.0.8/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.8/bin/logs/catalina.out
```

