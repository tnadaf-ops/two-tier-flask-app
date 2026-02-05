-pipeline{
    agent {label "dev"};
    stages{
        stage(code){
            steps{
                git url: "https://github.com/tnadaf-ops/two-tier-flask-app.git", branch: "master"
                echo "code cloned successfully"
            }
        }
        stage(build){
            steps{
                sh "docker build -t two-tier-flask-app ."
                echo "build successfully"
            }
        }
        stage(test){
            steps{
                echo "Test successfully"
            }
        }
        stage(dockerHub){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                echo "push to docker hub successfully"
                }
            }
        }
        stage(deploy){
            steps{
                sh "docker compose up -d --build flask-app"
                echo "code deploy successfully"
            }
        }
    }
    post{
        success{
            script{
                emailext from: "nadaftousief@gmail.com,
                subject: "Jenkins Build",
                body: "Good news: Jenkins build successfully",
                to: "nadaftousief@gmail.com"    
            }
        }
    }
}
