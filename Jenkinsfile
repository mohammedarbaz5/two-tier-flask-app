pipeline{
    agent { label "dev" };
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/mohammedarbaz5/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                echo "Yeh hai Arbaz on Jenkins"
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Developer / Tester tests bhi likh ke dega"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCredentials",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Docker compose se deploy hogya"
                sh '''
                    docker rm -f mysql flask-app || true
                    docker volume rm demo-cicd_mysql-data || true
                    docker compose up -d --build flask-app
                    docker update --memory 400m --memory-swap 800m mysql || true
                '''
            }
        }
    }
}   
