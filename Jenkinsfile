pipeline {
    agent any
    environment {

        EMAIL_BODY =
        """
            <p>EXECUTED: Job <b>\'${env.JOB_NAME}:${env.BUILD_NUMBER})\'</b></p>
            <p>
            View console output at
            "<a href="${env.BUILD_URL}">${env.JOB_NAME}:${env.BUILD_NUMBER}</a>"
            </p>
            <p><i>(Build log is attached.)</i></p>
        """

        EMAIL_SUBJECT_SUCCESS = "Status: 'SUCCESS' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'"
        EMAIL_SUBJECT_FAILURE = "Status: 'FAILURE' -Job \'${env.JOB_NAME}:${env.BUILD_NUMBER}\'"
        EMAIL_RECEPIENT = 'naniyule@gmail.com'


    tools {
        nodejs 'Node-19.8.0'
    }
    
        
    stages {
        stage('Start') {
            steps {
                echo 'Build is starting'
            }
        }
        stage ('Clone repository') {
            steps {
                git url: 'https://github.com/Naniyule/gallery.git', branch: 'main'
            }
        }

        stage ('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

         stage ('Run tests') {
             steps {
                 sh 'npm test'
             }
         }
        
         stage('Build'){
            steps{
                echo('Build successful')
            }
        }
         }
    }

          stage('End') {
            steps {
                echo 'Deployment completed'
            }
        }
    }

    post{
        failure {
            emailext attachLog: true, 
                body: EMAIL_BODY, 
                subject: EMAIL_SUBJECT_FAILURE, 
                to: EMAIL_RECEPIENT
        }
        success {
            slackSend channel: '#sydneyip1',
                        color: 'good',
                        message: "The pipeline ${currentBuild.fullDisplayName} completed successfully. Visit https://gallery-app-1ho8.onrender.com "
        }
    }     
}

}
