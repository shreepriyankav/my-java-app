pipeline {
    agent any

    environment {
        APP_NAME = 'my-java-app'
        IMAGE_NAME = 'my-java-app:1.0'
        CONTAINER_NAME = 'my-java-app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Dependency Check') {
            steps {
                sh '''
                    rm -rf target/dependency-check-report
                    mkdir -p target/dependency-check-report

                    /opt/dependency-check-12.1.0/bin/dependency-check.sh \
              --project "my-java-app" \
              --scan target/myapp.war \
              --format XML \
              --out target/dependency-check-report \
              --disableOssIndex \
              --failOnCVSS 11
        '''
    }
}
        stage('Docker Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh '''
                    trivy image \
                      --scanners vuln \
                     
                      --format json \
                      --output trivy-report.json \
                      ${IMAGE_NAME}
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true

                    docker run -d \
                      --name ${CONTAINER_NAME} \
                      -p 8081:8080 \
                      ${IMAGE_NAME}
                '''
            }
        }

        stage('Application Test') {
            steps {
                sh '''
                    sleep 5
                    curl -f http://localhost:8081/hello
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/dependency-check-report/dependency-check-report.xml,trivy-report.json',
                             allowEmptyArchive: true
        }

        success {
            echo '======================================'
            echo 'PIPELINE COMPLETED SUCCESSFULLY'
            echo 'Application: http://EC2-PUBLIC-IP:8081/hello'
            echo '======================================'
        }

        failure {
            echo 'PIPELINE FAILED - CHECK THE CONSOLE OUTPUT'
        }
    }
}
