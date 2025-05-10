pipeline{
    agent any;
    stages{
        stage("CODE"){
            steps{
                git url :"https://github.com/Karandeep9045/two-tier-flask-app.git" , branch: "master"
                echo "code clone ho gya..."
            }
        }
        stage("BUILD"){
            steps{
                sh "docker build -t two-tier-flask-app . "
                echo "Image buils ho gyi..."
            }
        }
        stage("TEST"){
            steps{
                echo "code testing done..."
            }
        }
        stage("PUSH TO HUB"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser"
                    )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                echo "docker hub pr push ho gyi..."
                }
            }
        }
        stage("DEPLOY"){
            steps{
                sh "docker compose up -d --build flask-app"
                echo "Deployement bhi ho gyi..."
            }
        }
    }
}
