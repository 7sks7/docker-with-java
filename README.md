# Containerizing a Spring Boot Application with Docker

This guide covers how to build, run, and publish a Docker image for a Spring Boot application using both **Spring Boot's built-in support** and **Google Jib**.

---

## 🚀 Building the Docker Image

### Using Spring Boot's `bootBuildImage`

1. **Build the JAR and Docker image:**

    - **For Gradle:**
      ```bash
      ./gradlew bootBuildImage
      ```
    - **For Maven:**
      ```bash
      ./mvnw spring-boot:build-image
      ```

2. **Run the container:**
   ```bash
   docker run -p 8080:8080 ApplicationName:0.0.1-SNAPSHOT

-> The first 8080 is your host port.
-> The second 8080 is the container's exposed port (used by Spring Boot).

3. Run the container in detached mode:
 docker run -d -p 8080:8080 ApplicationName:0.0.1-SNAPSHOT
-> This prints the container ID and runs it in the background.


**Publishing the Image to Docker Hub**
1. Tag the image:
docker tag containerize:0.0.1-SNAPSHOT DockerHubUserName/AppName:latest

2. Log in to Docker:
docker login
-> This will prompt you to confirm via browser.

3. Push the image:
docker push DockerHubUserName/AppName:latest


📥 Pulling and Running the Image on Another System
1. Pull the image:
docker pull DockerHubUserName/AppName
e.g.
docker pull vickypers/mycontainerizeapp

2. Run the container:
docker run -d -p 8080:8080 DockerHubUserName/AppName


🐳 Using Google Jib (Without Docker Installed)
1. Add Jib plugin to build.gradle:

plugins {
    id 'com.google.cloud.tools.jib' version '3.4.5'
    }

jib {
    from {
        image = 'eclipse-temurin:21-jre'
    }
    to {
        image = 'vickypers/mycontainerizeapptest'
    }
}

2. Build and publish the image to Docker Hub:
./gradlew build jib

3. Run the container:
docker run -d -p 8080:8080 vickypers/mycontainerizeapptest
-> If the image isn't present locally, Docker will automatically pull it.

🧪 Build & Load Docker Image Locally (Without Publishing)
-> If you want to build the Docker image and load it directly into your local Docker Desktop without pushing it to Docker Hub:
1. Build image and load into local Docker Desktop:

./gradlew jibDockerBuild
-> This skips Docker Hub and loads the image into your local Docker daemon.

Example output:
Built image to Docker daemon as vickypers/mycontainerizeapptest

-> This skips the remote registry and adds the image directly to your local Docker daemon.