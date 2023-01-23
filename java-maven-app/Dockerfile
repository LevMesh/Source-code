
#Configuring the image and specify it as a build.

FROM maven:3.8-openjdk-18 AS build

#Setting the WORKDIR to app and entering it.

WORKDIR /app

#Copying the directory src from your current location to inside the container to app/src.

COPY . .

#Copying the pox.xml into the container current directory (app).

COPY pom.xml .

#Installing all the dependecies with the command mvn verify.

RUN mvn package

#----------------------------------------------
########### Multi staging docker #############
#----------------------------------------------

FROM openjdk:8-jre-slim AS Runner

#Copying the directory app/target from our build which was created thanks to the stage - RUN mvn verify into a new directory inside the container /app/multi-stage/

WORKDIR /app/multi-stage

COPY --from=build /app/target .


#RUN /app/multi-stage/target/*.war /usr/local/tomcat/webapps/wwp-1.0.0.war
#Executing the file hello-java.jar 

ENTRYPOINT ["java", "-jar", "./java-maven-app-1.1.0-SNAPSHOT.jar"]

#ENTRYPOINT ["sleep" , "12000"]



# FROM tomcat9
# RUN /target/*.war /usr/local/tomcat/webapps/wwp-1.0.0.war