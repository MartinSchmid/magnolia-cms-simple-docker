FROM debian:stretch

MAINTAINER Martin Schmid "https://github.com/martinschmid"

ENV MYSQL_JDBC_PACKAGE mysql-connector-java-5.1.36
ENV POSTGRESQL_JDBC_PACKAGE postgresql-9.4-1201.jdbc41
ENV DEBIAN_FRONTEND noninteractive
ENV LANG C.UTF-8

# ENV DCEVM_VERSION dcevm-light-jdk7u71-2
#RUN echo "deb http://ftp.de.debian.org/debian wheezy-backports main " >> /etc/apt/sources.list 

# Install DCEVM
RUN apt-get update 
RUN apt-get upgrade -y
RUN apt-get install -y openjdk-8-jdk 
RUN apt-get install unzip wget tomcat8 libservlet3.1-java -y

# Fix certificate issues, found as of 
# https://bugs.launchpad.net/ubuntu/+source/ca-certificates-java/+bug/983302
RUN apt-get update && \
	apt-get install -y ca-certificates-java && \
	apt-get clean && \
	update-ca-certificates -f && \
	rm -rf /var/lib/apt/lists/* && \
	rm -rf /var/cache/oracle-jdk8-installer;

# Setup JAVA_HOME, this is useful for docker commandline
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME

# S
# Setup maven for Magnolia
# RUN mvn org.sonatype.plugins:nexus-m2settings-maven-plugin:1.6.5:download -DnexusUrl=https://nexus.magnolia-cms.com -Dusername=anonymous -Dpassword=anonymous -DtemplateId=magnolia-community-public > /dev/null 

# Install hot swap
# COPY ./jre/hotswap-agent.jar /usr/local/lib/

COPY tomcat/setenv.sh $CATALINA_BASE/bin/setenv.sh
COPY tomcat/ROOT.xml $CATALINA_BASE/conf/Catalina/localhost/ROOT.xml

COPY tomcat/start-debug.sh $CATALINA_BASE/bin/start-debug.sh
RUN chmod +x $CATALINA_BASE/bin/start-debug.sh

# COPY aardwark/aardwark.properties $CATALINA_BASE/webapps/aardwark.properties
# COPY aardwark/aardwark.war $CATALINA_BASE/webapps/aardwark.war

# Exposes JMX and JPDA port for debugging from an IDE
#EXPOSE 1043 1099

#ENTRYPOINT ["/var/lib/tomcat8/bin/start-debug.sh"]
