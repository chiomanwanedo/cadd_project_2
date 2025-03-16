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
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "chiomanwanedo"
                   git config --global user.email "chiomavanessa08@gmail.com"
                   git add deployment.yaml
                   git commit -m "Updated Deployment Manifest"
                   git pull --rebase origin main  # Fetch latest changes before pushing
                   git push origin main
                """
            }
        }
    }
}
