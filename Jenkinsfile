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
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/chiomanwanedo/cadd_project_2.git'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   echo "Before update:"
                   cat deployment.yaml

                   echo "Updating image tag..."
                   sed -i 's#${APP_NAME}:.*#${APP_NAME}:${IMAGE_TAG}#g' deployment.yaml

                   echo "After update:"
                   cat deployment.yaml

                   echo "Checking for changes..."
                   git diff --exit-code || echo "Changes detected!"
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh """
                        set -x  # Enable debugging

                        git config --global user.name "chiomanwanedo"
                        git config --global user.email "chiomavanessa08@gmail.com"

                        # Ensure the branch is up to date
                        git fetch origin main
                        git checkout main
                        git reset --hard origin/main

                        # Add and commit only if there are changes
                        if git diff --quiet; then
                          echo "No changes detected, skipping commit."
                          exit 0
                        fi

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
