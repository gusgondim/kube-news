pipeline {
    agent any
        stages {
            stage('build docker image') {
                steps {
                    script {
                        dockerapp = docker.build("gusgondim/kube-news:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                    }
                }
            }
        }
}