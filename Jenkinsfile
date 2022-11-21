pipeline {
    agent any
    tools {
        nodejs "nodejs"
    }

    environment 
        {
        APP_LINK = 'https://gallery-app-ip1.herokuapp.com/'
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
             emailext body: 'Tests failed', recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']], subject: 'Test'
            }
        }
        }
        stage('Deploy to heroku'){
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'heroku', variable: 'HEROKU_CREDENTIALS' )]){
                    sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/gallery-app-ip1.git master'
                }
            }
            post {
                success {
                    slackSend color: "good", message: "Deploy successful. Build ${BUILD_ID}, heroku link: ${APP_LINK}"
                }
            }
        }
    }
}