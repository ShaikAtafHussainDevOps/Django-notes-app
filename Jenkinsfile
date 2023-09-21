pipeline{
    agent any
    stages{
        stage("git cloning"){
            steps{ 
                echo "Cloning the project from github"
                git url:"https://github.com/ShaikAtafHussainDevOps/Django-notes-app.git", branch:"master"            
                }
        }
        stage("Build"){
            steps{ 
                echo "Building the code"
                sh 'docker build -t djangonotesappimg1:v1 .'
            }
        }
         stage("Push to docker hub"){
            steps{ 
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag djangonotesappimg1:v1 ${env.dockerHubUser}/djangonotesappimg1:v1"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/djangonotesappimg1:v1"
                }
            }
        }
        stage("Deploy"){
            steps{ 
                echo "Deploying the code"
                sh 'docker-compose down && docker-compose up -d'
                sh 'docker image prune -a'
            }
        }
    }
}