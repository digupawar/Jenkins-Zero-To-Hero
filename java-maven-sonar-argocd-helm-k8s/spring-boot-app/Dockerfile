# You can change this base image to anything else
# But make sure to use the correct version of Java
FROM adoptopenjdk/openjdk11:alpine-jre

# Simply the artifact path
#Refer to Maven build -> finalname
#https://github.com/digupawar/Jenkins-Zero-To-Hero/blob/main/java-maven-sonar-argocd-helm-k8s/spring-boot-app/pom.xml#L54
ARG artifact=target/spring-boot-web.jar

# cd /opt/app
WORKDIR /opt/app

# cp target/spring-boot-web.jar /opt/app/app.jar
COPY ${artifact} app.jar

# This should not be changed
# java -jar /opt/app/app.jar
ENTRYPOINT ["java","-jar","app.jar"]
