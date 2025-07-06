pipeline{
    
    agent { label "dev" };
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/rushichavan18/two-tier-flask-app.git" , branch: "master"
             }
         }
            stage("Build"){
                steps{
                   sh "docker build -t two-tier-flask-app ." 
                }
            }
            stage("Test"){
                steps{
                    echo"Code Test ho gaya"
                }
            }
            stage ("Push to Docker Hub"){
                steps{
                    withCredentials([usernamePassword(
                        credentialsId:"dockerHubCreds",
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubUser"
                        )]){
                        
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                    }
                }
            }
            stage("Deploy"){
                steps{
                    sh "docker compose up -d --build flask-app"
                }
            }
        }
        
post {
    success {
        script {
        emailext from: "rushihavan180501@gmail.com",
            subject: "Demo app has been updated and deploy Successful",
            body: "Build Successful",
        to: "rc8621658@gmail.com"
        }
    }    
    failure {
        script {
        emailext from: "rushihavan180501@gmail.com",
            subject: "Demo app Build Failed",
            body: "Build Unsuccessful",
        to: "rc8621658@gmail.com"
        }
    }
}        
    }
