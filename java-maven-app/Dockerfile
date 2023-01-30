
#Configuring the image and specify it as a build.

FROM maven:3.8-openjdk-18 AS build

#Setting the WORKDIR to app and entering it.

WORKDIR /app

#Copying everything in the current location into inside the container to /app.

COPY . .

#Installing all the dependecies with the command mvn package.

RUN mvn package

#----------------------------------------------
########### Multi staging docker #############
#----------------------------------------------

FROM openjdk:8-jre-slim AS Runner

#Copying the directory app/target from our build which was created thanks to the stage - RUN mvn package into a new directory inside the container /app/multi-stage/

WORKDIR /app/multi-stage

COPY --from=build /app/target .

#Executing the file java-maven-app-1.1.0-SNAPSHOT.jar.

ENTRYPOINT ["java", "-jar", "./java-maven-app-1.1.0-SNAPSHOT.jar"]

