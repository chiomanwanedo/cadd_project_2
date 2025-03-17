pipeline {
    agent { label "jenkins-agent" }
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
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Ashfaque-9x/gitops-register-app'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh """
                        git config --global user.name "chiomanwanedo"
                        git config --global user.email "chiomavanessa08@gmail.com"
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest"
                        git pull --rebase origin main
                        git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/chiomanwanedo/cadd_project_2.git main
                    """
                }
            }
        }
    }
}
