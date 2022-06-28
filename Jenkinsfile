// Declarative pipeline
pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "6281600/app"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }

    stages {

        /* We do not need a stage for checkout here since it is done by default when using "Pipeline script from SCM" option. */   
        
        stage ('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                bat 'docker build -t %DOCKER_HUB_REPO%:%BUILD_NUMBER% .'       
            }
        }
        stage ('Creating Docker Container') {
            steps {
                echo 'Creating Docker Container using %DOCKER_HUB_REPO%'
                bat 'docker run -td --name="app%BUILD_NUMBER%" -p 80%BUILD_NUMBER%:80 %DOCKER_HUB_REPO%:%BUILD_NUMBER%'       
            }
        }
        stage ('Testing Docker Container') {
            steps {
                echo 'Testing HTTP Response'
                bat 'curl localhost:80%BUILD_NUMBER%'       
            }
        }
        stage ('Push Image ') {
            steps {
                echo 'Pushing Image'
                withDockerRegistry(credentailsId: 'docker_cred' , url: '') {
                bat 'docker push %DOCKER_HUB_REPO%:%BUILD_NUMBER%'
            }
        }
    }
}
