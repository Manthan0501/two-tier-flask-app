pipeline{
    agent {label "dev"};
    stages{
        stage("code clone"){
            steps{
            git url: "https://github.com/Manthan0501/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("code build"){
            steps{
                sh "docker build -t notes-app:latest ."
            }
        }
        stage("push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "docker_hub",
                    passwordVariable: "password",
                    usernameVariable: "username"
                    )]){
                        sh "docker login -u ${env.username} -p ${env.password}"
                        sh "docker image tag notes-app ${env.username}/notes-app"
                        sh "docker push ${env.username}/notes-app:latest"
                    
                }
                
            }
        }
        stage("code deploy"){
            steps{
                sh "docker compose up -d --build"
            }
        }
    }
 post{
     success{
            mail from: "tiwarimanthan0501@gmail.com",
            to: "tiwarimanthan0501@gmail.com",
            subject: "build successful",
            body: "your build is successful"
     }
     failure{
            mail from: "tiwarimanthan0501@gmail.com",
            to: "tiwarimanthan0501@gmail.com",
            subject: "build failure",
            body: "your build is failure"
         
     }
 }   
}
