pipeline {
    agent any
    environment{
        APP_NAME = "myapplication"
    }
    stages {
         stage("Cleanup Workspace") {
             steps {
                cleanWs()
             }
         }
         stage("Checkout from SCM") {
             steps {
                     git branch: 'main', credentialsId: 'Git-hub', url: 'https://github.com/Srikanth141/myapplication-cd.git'
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
         stage("Push the changed deployment file to GitHub") {
            steps {
                sh """
                    git config --global user.name "srikanth"
                    git config --global user.email "srikanth.java1411@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'Git-hub', gitToolName: 'Default')]) {
                    sh "git push https://github.com/Srikanth141/myapplication-cd main"
                }
            }
         }
    }
}

    
