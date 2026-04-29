pipeline {
    agent { label 'ci' }

    environment {
        APP_NAME = "SpringBoot-WebApplication"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/gucdeepthi/SpringBoot-WebApplication.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh '''
                  echo "Running on CI agent"
                  mvn clean test
                '''
            }
        }

        stage('Package') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
    }


    post {
        success {
            build job: 'CD_pipeline',
                 wait: false,
                 parameters: [
                    string(name: 'IMAGE_TAG', value: "${BUILD_NUMBER}")
                    ]
         }
        failure {
            echo "❌ CI failed"
        }
    }
}
