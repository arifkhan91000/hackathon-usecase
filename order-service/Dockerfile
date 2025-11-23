# Build stage: use Maven to build the jar
FROM maven:3.8.7-eclipse-temurin-17 AS build
WORKDIR /workspace

# copy pom and download dependencies (cache)
COPY pom.xml .
# If multi-module you may need to COPY the whole pom structure first
RUN mvn -B -DskipTests dependency:go-offline

# copy the source and build
COPY src ./src
RUN mvn -B clean package -DskipTests

# Run stage: smaller runtime image
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app

# copy built jar from build stage — adjust the jar name/path as produced by your pom
COPY --from=build /workspace/target/*.jar app.jar

ENV JAVA_OPTS=""
EXPOSE 8080

ENTRYPOINT ["sh","-c","java $JAVA_OPTS -jar /app/app.jar"]
