pipeline {
       agent {
        docker {
            image 'node:lts-bullseye-slim' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build the project') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Run Tests'){
            steps {
                sh 'npm test'
            }
        }
        stage('Deploy to heroku'){
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'heroku', variable: 'HEROKU_CREDENTIALS' )]){
                    sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/gallery-app-ip1.git master'
                }
            }

        }
    }
}