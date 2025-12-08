pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/yyz18/gitops-register-app'
               }
        }

       

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "yyz18"
                   git config --global user.email "mozubayer@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/yyz18/gitops-register-app main"
                }
            }
        }
      
    }
}
