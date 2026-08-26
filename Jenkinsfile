@Library("Shared") _
pipeline {
    
    agent { label "dev"};

    stages{
        stage("code clone"){
            steps{
                script{
                    clone("https://github.com/Bhushan-88/two-tier-flask-app.git", branch: "master")
                }
            }
        }
        stage("trivy scan"){
            steps{
                sh "trivy fs . -o results.json"
                echo "trivy scan done"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
                echo "docker build stage success"
            }
            
        }
        stage("test"){
            steps{
                echo "test tester will give"
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    withCredentials([usernamePassword(
                        credentialsId: "dockerHubCreds"
                        , passwordVariable: "dockerHubPass"
                        , usernameVariable: "dockerHubUser"
                        )]) {
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app:latest"
                        sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                    
                    }
                }  
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
                echo "deploy by docker compose"
            }
            
        }
    }
    post{
        success{
            script{
                emailext from: 'bhushandurgawli1@gmail.com',
                to: 'bhushandurgawli1@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'bhushandurgawli1@gmail.com',
                to: 'bhushandurgawli1@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
