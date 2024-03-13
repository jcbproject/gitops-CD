Pipeline {
    agent{ label "Jenkis-Agent" }
    environment {
        APP_NAME = "register-app-pipeline"
    }

    stages {
        stage ("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }

        stage ("Checkout fron SCM"){
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/jcbproject/gitops-CD.git'
            }
        }
        stage("Update the Deployment Tags"){
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }
        stage("Push the changed deployment file to Git"){
            steps{
                 sh """
                    git config --global user.name "jcbproject"
                    git config --global user.email "juan.barreto18@gmail.com"
                    git add deployment.yaml
                    git commit -m "Update Deployment Manifest"
                """
                 withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'default')]){
                     sh "git push https://github.com/jcbproject/gitops-CD.git main"
                 }
            }
        }
    }        
}