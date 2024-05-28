# You can change this base image to anything else
# But make sure to use the correct version of Java
FROM adoptopenjdk/openjdk11:alpine-jre

# Simply the artifact path
#Refer to Maven build -> finalname
ARG artifact=target/spring-boot-web.jar

# cd /opt/app
WORKDIR /opt/app

# cp target/spring-boot-web.jar /opt/app/app.jar
COPY ${artifact} app.jar

# This should not be changed
# java -jar /opt/app/app.jar
ENTRYPOINT ["java","-jar","app.jar"]
