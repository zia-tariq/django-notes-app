pipeline {
    agent any
    
    stages {
        stage ("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/zia-tariq/django-notes-app.git", branch: "main"
                
            }
            
        }
        stage ("Build") {
            steps {
                echo "Building the image"
                sh "docker build . -t my-note-app"
                
            }
            
        }
        stage ("Push to Docker Hub") {
            steps {
                echo "Pushing the image to docker hub"
                //We use credentialsId we added in credentials in manage Jenkins
                withCredentials([usernamePassword(credentialsId:'dockerHub', passwordVariable:'dockerHubPass', usernameVariable: 'dockerHubUser')]) {
                //Now we tag our note app above with our docker hub usernmae/name of app
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                //login to Docker Hub
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                //push image to docker hub with username/name of image
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                
            }
            
        }
        
    }
        stage ("Deploy") {
            steps {
                echo "Deploying the container"
                //run image we push on docker hub in above step. We should run it with docker-compose as if after 1st run we run container again it will says port already in use
               // sh "docker run -d -p 8000:8000 ziatariq2023/my-note-app:latest"
               sh "docker-compose down && docker-compose up -d"
                
            }
            
        }
    }
    
}
