# My Java App – CI/CD Pipeline

## Project Overview

This project demonstrates a complete CI/CD pipeline for a Java web application using AWS EC2, GitHub, Jenkins, Maven, SonarQube, OWASP Dependency-Check, Docker, and Trivy.

The Java application is packaged as a WAR file and deployed inside a Docker container running Tomcat.

## CI/CD Flow

```text
AWS EC2 Ubuntu
      |
      v
GitHub
      |
      v
Jenkins
      |
      v
Maven Build
      |
      v
SonarQube Analysis
      |
      v
OWASP Dependency-Check
      |
      v
Docker Build
      |
      v
Trivy Image Scan
      |
      v
Docker Container
      |
      v
Application Test
      |
      v
Java Web Application
      |
      v
http://EC2-PUBLIC-IP:8081/hello
```

## Technologies Used

| Technology             | Purpose                             |
| ---------------------- | ----------------------------------- |
| AWS EC2                | Cloud server                        |
| Ubuntu                 | Operating system                    |
| Git                    | Source code management              |
| GitHub                 | Remote code repository              |
| Jenkins                | CI/CD automation                    |
| Java 17                | Application development             |
| Maven                  | Build and package the application   |
| SonarQube              | Code quality analysis               |
| OWASP Dependency-Check | Dependency vulnerability scanning   |
| Docker                 | Containerization                    |
| Tomcat                 | Java web application server         |
| Trivy                  | Docker image vulnerability scanning |
| curl                   | Application testing                 |

## Application

The project is a simple Java Servlet application.

The servlet endpoint is:

```text
/hello
```

The application displays:

```text
Hello from Java Maven Docker Jenkins!

Application is running successfully.
```

## Project Structure

```text
my-java-app/
│
├── .git/
├── .gitignore
├── Dockerfile
├── Jenkinsfile
├── README.md
├── pom.xml
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── example/
│                   └── HelloServlet.java
│
└── target/
    └── myapp.war
```

## 1. AWS EC2

The application and CI/CD tools are hosted on an AWS EC2 Ubuntu instance.

The EC2 instance contains the required tools:

* Java
* Maven
* Git
* Jenkins
* Docker
* SonarQube
* OWASP Dependency-Check
* Trivy

## 2. Maven Build

Maven is used to compile the Java source code and create the WAR package.

Jenkins runs:

```bash
mvn clean package -DskipTests
```

The generated WAR file is:

```text
target/myapp.war
```

## 3. SonarQube Analysis

SonarQube is used to analyze the source code for code quality issues.

Jenkins runs the SonarQube analysis and sends the project information to the SonarQube server.

The project uses:

```text
Project Key: my-java-app
Project Name: my-java-app
```

The Jenkins pipeline successfully received:

```text
SonarQube Quality Gate: Passed
```

## 4. OWASP Dependency-Check

OWASP Dependency-Check scans project dependencies for known security vulnerabilities.

The pipeline generates:

```text
target/dependency-check-report/dependency-check-report.xml
```

The report is archived by Jenkins after the pipeline completes.

## 5. Docker Build

The generated Java application is packaged into a Docker image.

The image name used in this project is:

```text
my-java-app:1.0
```

Docker is used to provide a consistent runtime environment for the application.

## 6. Trivy Security Scan

Trivy scans the Docker image for vulnerabilities.

The Jenkins pipeline uses:

```bash
trivy image --scanners vuln --format json --output trivy-report.json "my-java-app:1.0"
```

The generated report is:

```text
trivy-report.json
```

Jenkins archives this report after the pipeline completes.

## 7. Docker Deployment

The Docker container is created using:

```bash
docker run -d \
  --name my-java-app \
  -p 8081:8080 \
  my-java-app:1.0
```

Port mapping:

```text
EC2 Port 8081
      |
      v
Docker Container Port 8080
      |
      v
Tomcat
      |
      v
Java Application
```

## 8. Application Test

Jenkins automatically tests the deployed application using:

```bash
curl -f http://localhost:8081/hello
```

If the application responds successfully, the pipeline continues successfully.

The application can also be accessed from a browser:

```text
http://EC2-PUBLIC-IP:8081/hello
```

## 9. Jenkins Pipeline Stages

The Jenkinsfile contains these stages:

```text
Checkout
   |
   v
Maven Build
   |
   v
SonarQube Analysis
   |
   v
Dependency Check
   |
   v
Docker Build
   |
   v
Trivy Scan
   |
   v
Deploy
   |
   v
Application Test
```

## 10. Jenkins Pipeline Result

The completed pipeline successfully achieved:

* GitHub checkout — SUCCESS
* Maven build — SUCCESS
* WAR generation — SUCCESS
* SonarQube analysis — SUCCESS
* SonarQube Quality Gate — PASSED
* OWASP Dependency-Check — SUCCESS
* Docker image build — SUCCESS
* Trivy scan — SUCCESS
* Docker deployment — SUCCESS
* Application test — SUCCESS

Jenkins archived:

```text
dependency-check-report.xml
trivy-report.json
```

## 11. Final Result

The Java application is successfully running inside a Docker container on AWS EC2.

Application URL:

```text
http://EC2-PUBLIC-IP:8081/hello
```

Expected output:

```text
Hello from Java Maven Docker Jenkins!

Application is running successfully.
```

## 12. What I Learned

This project demonstrates the complete flow of a basic DevSecOps CI/CD pipeline:

```text
Code
  |
  v
GitHub
  |
  v
Jenkins
  |
  +--> Maven Build
  |
  +--> SonarQube Code Quality
  |
  +--> OWASP Dependency Security Scan
  |
  +--> Docker Image Build
  |
  +--> Trivy Container Security Scan
  |
  +--> Docker Deployment
  |
  +--> Application Testing
  |
  v
Running Java Application
```

This project helped demonstrate how development, build automation, code quality, security scanning, containerization, deployment, and automated testing can be combined into one CI/CD pipeline.
# my-java-app
