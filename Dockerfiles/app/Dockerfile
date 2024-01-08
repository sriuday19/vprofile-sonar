FROM tomcat:9-jre11-openjdk
RUN apt-get update -y
RUN rm -rf /var/lib/tomcat/webapps/*
RUN /var/lib/jenkins/vprofile-sonar/target/vprofile-v2.war /var/lib/tomcat/webapps/ROOT.war
CMD [ "catalina.sh", "run" ]