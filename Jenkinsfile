pipeline {
    agent any

    tools {
        maven 'Maven3'   // Make sure Maven is configured in Jenkins
        jdk 'JDK11'      // Make sure JDK is configured in Jenkins
    }

    environment {
        APP_DIR = "/home/ubuntu/jenkins-practice/springboot-demo-nexus-/springboot-nexus-demo"
        JAR_NAME = "springboot-nexus-demo-1.0.0.jar"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/das28/springboot-demo-nexus-.git'
            }
        }

        stage('Unzip Project') {
            steps {
                sh """
                cd $APP_DIR
                unzip -o springboot-nexus-demo.zip
                """
            }
        }

        stage('Build') {
            steps {
                sh """
                cd $APP_DIR/springboot-nexus-demo
                mvn clean package
                """
            }
        }

        stage('Run App') {
            steps {
                sh """
                cd $APP_DIR/springboot-nexus-demo/target
                # Stop any previous instance running on 8080
                fuser -k 8080/tcp || true
                # Run app in background
                nohup java -jar $JAR_NAME > app.log 2>&1 &
                """
            }
        }
    }

    post {
        success {
            echo "Spring Boot app deployed successfully!"
        }
        failure {
            echo "Build or deployment failed. Check console logs."
        }
    }
}

