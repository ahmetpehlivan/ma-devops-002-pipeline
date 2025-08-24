pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {

       stage('SCM GitHub') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ahmetpehlivan/ma-devops-002-pipeline']])
            }
        }

    stage('test Maven') {
            steps {
            //    sh 'mvn clean install'
                bat 'mvn test'
            }
        }

        stage('Build Maven') {
            steps {
            //    sh 'mvn clean install'
                bat 'mvn clean install'
            }
        }

        stage('Docker Image') {
            steps {
            //    sh 'mvn clean install'
                bat 'docker build -t ahmetpehlivan/ma-devops-002-app:latest .'
            }
        }


         stage('Docker Image To DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub_token', variable: 'MY_DOCKERHUB_TOKEN')]) {
                         if (isUnix()) {
                             sh 'docker login -u ahmetpehlivan -p %MY_DOCKERHUB_TOKEN%'
                             sh  'docker push ahmetpehlivan/ma-devops-002-app:latest'
                          } else {
                             bat 'docker login -u ahmetpehlivan -p %MY_DOCKERHUB_TOKEN%'
                             bat  'docker push ahmetpehlivan/ma-devops-002-app:latest'
                         }

                    }
                }
            }
        }

        stage('Deploy K8s') {
            steps {
               kubernetesDeploy configs: 'deployment-service.yml', kubeconfigId: 'kubernetes')
            }
        }

    }
}