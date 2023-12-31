pipeline {
    agent{ label 'Jenkins-Agent' }
    environment{
            APP_NAME = "register-app-pipeline"
    }
    
    stages{
        stage("Cleanup Workspace"){
                steps{
                    cleanWs()
            }
        }
    

    stage("Checkout from SCM"){
        steps{
            git branch: 'main', credentialsId: 'github', url: 'https://github.com/SurendraAmmasi/gitops-register-app.git'
        }
    }

    stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                   cat deployment.yml
                """
            }
        }

    stage("Push the changed deployment file to Git") {
            steps {
                sh """
                   git config --global user.name "SurendraAmmasi"
                   git config --global user.email "surendrakumarammasi@gmail.com"
                   git add deployment.yml
                   git commit -m "Updated Deployment Manifest"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                  sh "git push https://github.com/SurendraAmmasi/gitops-register-app.git main"
                }
            }
        }        
    }
}
