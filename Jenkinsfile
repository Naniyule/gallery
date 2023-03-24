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
    }
    tools {
        nodejs "node 19.8.0"
    }
    stages {
        stage('Start') {
            steps {
                echo 'Build is starting'
            }
        }
        stage('Clone github repository') {
            steps {
                git url: 'https://github.com/Naniyule/gallery.git', branch: 'master'

            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test'){
            steps {
                sh 'npm test'
            }
        }
       
        stage('deploy to Render'){
            steps {
                sh 'curl -X POST https://api.render.com/deploy/srv-cgcnate4dad6fr5pob00?key=P8i_3dp_kWA'
           
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
                        message: "The pipeline ${currentBuild.fullDisplayName} completed successfully. Visit https://gallery-rkb6.onrender.com/ "
        }
    }     
}

