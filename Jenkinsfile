pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK11'
    }

    environment {
        APP_DIR = "${WORKSPACE}"    // use Jenkins workspace dynamically
        JAR_NAME = "springboot-nexus-demo-1.0.0.jar"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/das28/springboot-demo-nexus.git'
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
                fuser -k 8080/tcp || true
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

