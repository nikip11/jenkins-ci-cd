pipeline {
    /*agent {
      label 'kubepods'
    }*/
    agent any
    environment {
        DOCKER_IMAGE_NAME = "nikip11/jenkins-ci-cd"

    }
    stages {
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps {
                milestone(1)
                script {
                    //git (branch: 'main',
                    //credentialsId: 'github_key_userpass',
                    //url: 'https://github.com/jenkinsci/pipeline-examples.git')

                    //sh "ls -lat"
                    kubernetesDeploy(
                        configs: "php-deployment.yaml",
                        kubeconfigId: "aks_kubeconfig",
                        enableConfigSubstitution: true
                        )
                }
            }
        }
    }
}
