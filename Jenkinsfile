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

                # Add changes only if they exist
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
