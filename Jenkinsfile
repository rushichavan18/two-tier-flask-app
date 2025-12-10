pipeline{
    
    agent { label "dev"};
    
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/rushichavan18/two-tier-flask-app.git", branch: "master"
            }
        }
        stage ("Trivy File System Scan"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Test"){
            steps{
                echo "Developer / Tester tests likh dega.."
            }
        }
        stage("Push to Docker Hub"){ 
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                sh "docker login -u ${env.dockerHubUSer} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app"
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
        script{
            emailext from: 'rushichavan180501@gmail.com',
             to: 'rc8621658@gmail.com',
             body: 'Build success for Demo CICD App',
             subject: 'Build success'
            }
        }
    failure {
        script{
            emailext from: 'rushichavan180501@gmail.com',
             to: 'rc8621658@gmail.com',
             body: 'Build Failed for Demo CICD App',
             subject: 'Build Failed'
            }
        }
}
}
