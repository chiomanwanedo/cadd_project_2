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
                   sed -i 's#${APP_NAME}.*#${APP_NAME}:${IMAGE_TAG}#g' deployment.yaml
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

                        # Ensure the branch is up to date
                        git fetch origin main
                        git checkout main
                        git reset --hard origin/main

                        # Add the updated file and commit
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest"

                        # Pull latest changes and rebase safely
                        git pull --rebase origin main || git rebase --abort

                        # Push the changes after successful rebase
                        git push https://$GIT_USERNAME:$GIT_PASSWORD@github.com/chiomanwanedo/cadd_project_2.git main
                    """
                }
            }
        }
    }
}
