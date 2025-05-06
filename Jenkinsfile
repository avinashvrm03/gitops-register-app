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
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/avinashvrm03/gitops-register-app.git'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                script {
                    sh """
                    echo '[Before Update]'
                    grep 'image:' deployment.yaml || true
                    
                   #sed -i 's#\\(image: avinash0001/registration-app-pepeline:\\).*#\\1${IMAGE_TAG}#' deployment.yaml
                    sed -i 's#\\(image: avinash0001/registration-app-pipeline:\\).*#\\1${IMAGE_TAG}#' deployment.yaml

                    
                    echo '[After Update]'
                    grep 'image:' deployment.yaml || true
                    """
                }
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "avinashvrm03"
                   git config --global user.email "avinashvrm03@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/avinashvrm03/gitops-register-app.git main"
                }
            }
        }
      
    }
}
