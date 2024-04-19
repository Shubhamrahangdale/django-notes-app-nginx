pipeline{
    agent any 
    stages {
        stage ("code"){
            steps{
                echo "clone the code"
                git url: "https://github.com/Shubhamrahangdale/django-notes-app.git", branch:"main"
                
            }
        }
        stage ("build"){
            steps{
                echo "Build the image"
                sh "docker build -t notes-app ."
            }
        }
        stage ("push the code in github"){
            steps{
                echo "push image in github"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                  sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"
                  sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                  sh "docker push ${env.dockerHubUser}/notes-app:latest"
                    
                }
                
            }
        }
        stage("Deploy"){
            steps{
                echo "Deployment of container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
