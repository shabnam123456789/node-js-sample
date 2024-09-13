pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/shabnam123456789/node-js-sample.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    def image = docker.build("node-js-sample:${env.BUILD_ID}")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-credentials-id') {
                        def image = docker.image("node-js-sample:${env.BUILD_ID}")
                        image.push("latest")
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    kubernetesDeploy(
                        configs: 'kubernetes/deployment.yaml',
                        kubeconfigId: 'kubeconfig-id'
                    )
                }
            }
        }
    }
}
