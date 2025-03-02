# 🏗️ Stage 1: Build Spring Boot Application using Java 23
FROM eclipse-temurin:23-jdk AS build

# Set working directory
WORKDIR /app

# Copy Maven wrapper and project files
COPY mvnw ./
COPY .mvn/ .mvn/
COPY pom.xml ./

# Ensure the Maven wrapper is executable
RUN chmod +x mvnw

# Download dependencies (cached for faster builds)
RUN ./mvnw dependency:go-offline

# Copy the entire source code
COPY src ./src

# Build the Spring Boot application (skipping tests for faster build)
RUN ./mvnw clean package -DskipTests

# 🏃 Stage 2: Run the application with Java 23 JRE (lighter image)
FROM eclipse-temurin:23-jre

# Set working directory
WORKDIR /app

# Copy the built JAR file from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose port 8080 for external access
EXPOSE 8080

# Command to run the application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
