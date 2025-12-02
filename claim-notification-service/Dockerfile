# Stage 1: Build the Spring Boot application using Maven
FROM maven:3.8.4-openjdk-17 AS builder
WORKDIR /app
COPY . /app
RUN mvn clean package -DskipTests

# Stage 2: Create a smaller image for running the app
FROM openjdk:17
WORKDIR /app

# Create a non-root user
RUN groupadd -g 1000 appgroup && \
    useradd -u 1000 appuser -g appgroup && \
    chown appuser:appgroup /app

# Use non-root user
USER appuser

# Copy the built JAR from the builder stage
COPY --from=builder /app/target/claim-notification-service-1.0.jar /app/demo.jar

# Run the application
ENTRYPOINT ["java", "-jar", "/app/demo.jar"]
