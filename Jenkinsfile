@Library("Shared") _
pipeline {
    agent {label "workernode" }
    stages{
        stage("Hello"){
            steps{
                script{
                    sharedlib()                         
                }
            }
        }
        
        stage("Code"){
            steps{
                script{
                    clone("https://github.com/kallesha9845/django-notes-app.git","main")                         
                }

            }
            
        }
        stage("Build"){
            steps{
                echo "This is building the code"
                sh "docker build -t notes-app:latest ."
            }
            
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub..."
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCred",
                    passwordVariable: "dockerHubpass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    // Login to Docker Hub
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubpass}"

                    // Tag the image with your Docker Hub username
                    sh "docker image tag notes-app:latest kallesh9773/notes-app:latest"

                    // Push the image to Docker Hub
                    sh "docker push kallesh9773/notes-app:latest"
                }
                echo "Docker image pushed to Docker Hub successfully."
            }
        }
        stage("Deploy"){
            steps{
                echo "This is deploying the code"
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
