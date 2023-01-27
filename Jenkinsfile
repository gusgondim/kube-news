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
            stage('Push Docker Image...'){
                steps {
                    script {
                         docker.withRegistry('https://registry.hub.docker.com')
                            dockerapp.push('latest')
                            dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
            stage('Deploy kubernetes') {
                environment{
                    tag_version = "${env.BULD_ID} "
                }
                steps {
                    withKubeConfig([credentialsId: 'kubeconfig']){
                        sh 'sed-i "s/{{TAG}}/$tag_version/g ./k8s/deployment.yaml'
                        sh 'kubectl apply -f ./k8s/deployment.yaml'

                    }
                }

            }
        }
}