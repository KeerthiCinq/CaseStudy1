# Build Stage
FROM maven:3.9.8-amazoncorretto-17 AS build

# Set the working directory
WORKDIR /app

# Copy the Maven POM file and resolve dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy the application source code
COPY src ./src

# Build the application JAR
RUN mvn clean package -DskipTests

# Final Stage
FROM amazoncorretto:17

# Label the image
LABEL authors="Keerthiga"

# Set the working directory for the final image
WORKDIR /app

# Copy the built JAR file from the build stage
COPY --from=build /app/target/user-service-0.0.1-SNAPSHOT.jar user-service.jar

# Optionally, copy the application.yaml only if needed
COPY src/main/resources/application.yaml /app/application.yaml

# Expose the service port
EXPOSE 8081


# Set the entry point to run the JAR file, including the sandbox profile
ENTRYPOINT ["java", "-jar", "./user-service.jar", "--spring.config.additional-location=./application.yaml"]
