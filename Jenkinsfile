pipeline {
    agent any

    environment {
        // Overriding the port to 8081 dynamically to prevent conflict with Jenkins on 8080
        APP_PORT = '8081'
    }

    stages {
        stage('Checkout') {
            steps {
                // Pulling code from your GitHub repository
                git branch: 'main', url: 'https://github.com/amankainth/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                // Ensure the wrapper script is executable and run the Maven package
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to local environment...'
                // Stop any previously running instance on port 8081 to prevent port conflicts
                sh "fuser -k ${APP_PORT}/tcp || true"
                
                // Run the application in the background so Jenkins doesn't hang waiting for it
                sh "nohup java -jar -Dserver.port=${APP_PORT} target/spring-petclinic-*.jar > app.log 2>&1 &"
            }
        }
    }
}
