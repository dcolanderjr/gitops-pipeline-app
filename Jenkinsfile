pipeline {
    agent { label 'Jenkins-Agent' }
    environment { 
            APP_NAME = 'pipeline_project_app'
  }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/dcolanderjr/gitops-pipeline-app'
            }
        }

        stage('Update the Deployment Tags') {
            steps {
                sh """
                cat deployment.yaml
                sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                cat deployment.yaml
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/dcolanderjr/gitops-pipeline-app main"
            }
        }
