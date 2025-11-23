# -------------------------------
# Stage 1 : Build the JAR using Maven
# -------------------------------
FROM maven:3.8.7-eclipse-temurin-17 AS build
WORKDIR /workspace

# Copy pom.xml first to download dependencies
COPY pom.xml .

RUN mvn -B -DskipTests dependency:go-offline

# Copy the rest of the project
COPY src ./src

# Build the JAR
RUN mvn -B clean package -DskipTests

# -------------------------------
# Stage 2 : Run the application
# -------------------------------
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app

# Copy the jar from the build stage
COPY --from=build /workspace/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java","-jar","/app/app.jar"]
