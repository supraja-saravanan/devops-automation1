pipeline {
    agent any
    environment {
        dockerHubCredential = 'Docker_key'
    }
    tools{
        maven 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/supraja-saravanan/devops-automation1.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t supraja13/devops-integration .'
                }
            }
        }
        stage('Pushing Image') {
    steps {
        script {
            docker.withRegistry('https://registry.hub.docker.com', dockerHubCredential) {
                sh 'docker push supraja13/devops-integration'
            }
        }
    }
}

        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'Kubernetes_key')
                }
            }
        }
    }
}
