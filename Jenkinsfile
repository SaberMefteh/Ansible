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
                    credentialsId: 'demoAnsibleAppService'  // Make sure this matches the correct credential ID
                )]) {
                    azureWebAppPublish(
                        azureCredentialsId: 'demoAnsibleAppService', // Ensure this matches the credential ID too
                        resourceGroup: 'demo',
                        appName: 'demoAnsible',
                        filePath: '**/*.zip'
                    )
                }
            }
        }
    }
}
