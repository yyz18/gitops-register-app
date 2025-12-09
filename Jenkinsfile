pipeline {
    agent { label "Jenkins-Agent" }
    environment {
        APP_NAME = "register-app-pipeline"
        DOCKER_USER = "mozubayer"
    }

    stages {
        stage("Cleanup Workspace") {
            steps { cleanWs() }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github',
                    url: 'https://github.com/yyz18/gitops-register-app'
            }
        }

        stage("Update Image Tag") {
            steps {
                sh """
                   echo 'Before update:'
                   grep image deployment.yaml

                   sed -i "s|image: .*/${APP_NAME}:.*|image: ${DOCKER_USER}/${APP_NAME}:${IMAGE_TAG}|g" deployment.yaml

                   echo 'After update:'
                   grep image deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "yyz18"
                   git config --global user.email "mozubayer@gmail.com"

                   git add deployment.yaml
                   git commit -m "Update image tag to ${IMAGE_TAG}"
                """

                withCredentials([gitUsernamePassword(credentialsId: 'github')]) {
                    sh "git push https://\${GIT_USERNAME}:\${GIT_PASSWORD}@github.com/yyz18/gitops-register-app HEAD:main"
                }
            }
        }
    }
}
