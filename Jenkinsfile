pipeline{
    agent{
        label "JenkinsAgent"
    }
    
    environment{
        APP_NAME="mycompletepipeline"
        github_access_token= credentials("github_access_token")
        Jenkins_API_access_token = credentials("Jenkins_API_access_token")

    }

    stages{
        stage (" Cleanup Workspace"){
            steps{
                cleanWs()   // fonction prédefinie to clean the workspace

            }

        } 

        stage (" Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github_access_token', url: 'https://github.com/wasrazki/gitops-mycompletepipeline'
            }
        } 

        stage (" Updating the Deployemtn Tags"){
            steps{
                sh """
                    cat deployment.yaml 
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        } 

        stage (" Push the changed deployment file to Git"){
            steps{
                sh """
                    git config --global user.name "wasrazki"
                    git config --global user.email "razkiiwassim@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updating the image tag"
                """
                withCredentials([gitUsernamePassword(credentialsId:"github_access_token", gitToolName: 'Default')]){
                    sh "git push https://github.com/wasrazki/gitops-mycompletepipeline main"
                }

            }

        }

        





    

}

}