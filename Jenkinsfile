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
                withSonarQubeEnv(credentialsId:'sonar-api'){
                    sh 'mvn clean package sonar:sonar'
            }
         }     
        }
    }
}
