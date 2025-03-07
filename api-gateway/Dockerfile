# Use OpenJDK 23 as the base image for building
FROM openjdk:23-ea-slim AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the Gradle wrapper and necessary files
COPY mvnw mvnw
COPY .mvn .mvn
COPY pom.xml /app/

# Make the Maven wrapper executable
RUN chmod +x mvnw

# Download dependencies (this step caches dependencies in Docker layers)
RUN ./mvnw dependency:resolve

# Copy the source code
COPY src src

# Build the JAR file
RUN ./mvnw clean package -DskipTests && ls -la target/

# Use a minimal JDK image for running the application
FROM openjdk:23-ea-slim

# Set the working directory inside the container
WORKDIR /app

# Copy the built JAR file from the build stage
COPY --from=build /app/target/api-gateway-1.jar api-gateway.jar

# Expose the application port
EXPOSE 8085

# Run the Spring Boot application
ENTRYPOINT ["java", "-jar", "api-gateway.jar"]

