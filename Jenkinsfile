pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                withCredentials([azureServicePrincipal(
                    credentialsId: 'demoServicePrincipal'
                )]) {
                    azureWebAppPublish(
                        resourceGroup: 'demo',
                        appName: 'demoAnsible',
                        filePath: '**/*.zip'
                    )
                }
            }
        }
    }
}
