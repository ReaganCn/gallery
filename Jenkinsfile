pipeline {
    agent any
    tools {
        nodejs "nodejs"
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
            post {
            failure {
             emailext body: 'A Test EMail', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
            }
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