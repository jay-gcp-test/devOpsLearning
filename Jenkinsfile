pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = "auto-test:${BUILD_NUMBER}"
                    docker.build(imageName)
                }
            }
        }
    }
}