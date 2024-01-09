FROM openjdk-11-jdk AS BUILD_IMAGE
RUN apt update && apt install maven -y
RUN git clone https://github.com/sriuday19/vprofile-sonar.git
RUN cd vprofile-sonar && mvn install

FROM tomcat:8-jre11-openjdk
RUN apt-get update -y
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=BUILD_IMAGE vprofile-sonar/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
EXPOSE 8080
CMD [ "catalina.sh", "run" ]