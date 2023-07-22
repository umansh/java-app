pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/umansh/java-app.git", branch: "main"
            }
        }
        stage("maven test"){
            steps {     
                sh 'mvn test'
            }
        }
        stage("maven integration verify"){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage("static code analysis"){
            steps {
                withSonarQubeEnv(installationName: 'sonar-api', credentialsId: 'sonar-api') {
                    sh 'mvn clean package sonar:sonar'
            }
          }     
        }
        stage("maven install"){
            steps {     
                sh 'mvn clean install'
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t java-app ."
            }
        }
        stage("docker image scan"){
            steps {    
                withCredentials([usernamePassword(credentialsId:"docker",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                 sh """
                    trivy image ${env.dockerHubUser}/java-app:latest > scan.txt
                    cat scan.txt
                """
                }
               
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"docker",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag java-app ${env.dockerHubUser}/java-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/java-app:latest"
                }
            }
        }
         stage("Build"){
            steps {
                echo "Building the image"
                sh """
                docker rmi java-app
                docker rmi java-app ${env.dockerHubUser}/java-app:latest
                """
            }
        }
          

        
    }
    
}
