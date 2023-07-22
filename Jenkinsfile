pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/umansh/java-app.git", branch: "main"
            }
        }
    }
}
