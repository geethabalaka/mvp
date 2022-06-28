// Declarative pipeline
pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "6281600/app"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        http_proxy = 'http://127.0.0.1:3128/'
        https_proxy = 'http://127.0.0.1:3128/'
        ftp_proxy = 'http://127.0.0.1:3128/'
        socks_proxy = 'socks://127.0.0.1:3128/'
    }

    stages {

        /* We do not need a stage for checkout here since it is done by default when using "Pipeline script from SCM" option. */   
        
        stage ('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh 'docker build -t $DOCKER_HUB_REPO:$BUILD_NUMBER .'       
            }
        }
        stage ('Creating Docker Container') {
            steps {
                echo 'Creating Docker Container using $DOCKER_HUB_REPO'
                sh 'docker run -td --name="app$BUILD_NUMBER" -P 80$BUILD_NUMBER:80 $DOCKER_HUB_REPO:$BUILD_NUMBER'       
            }
        }
        stage ('Testing Docker Container') {
            steps {
                echo 'Testing HTTP Response'
                sh 'wget localhost:80$BUILD_NUMBER'       
            }
        }
        stage ('Push Image ') {
            steps {
                echo 'Pushing Image'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR  --password-stdin && docker push $DOCKER_HUB_REPO:$BUILD_NUMBER'
            }
        }
    }
}
