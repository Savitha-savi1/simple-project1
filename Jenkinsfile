pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET = 'jenkins-maven-deploy-demo'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[url: 'https://github.com/Savitha-savi1/simple-project1.git']]])
            }
        }
        stage('Deploy to S3') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws s3 sync . s3://$S3_BUCKET --delete --exclude ".git" --exclude "Jenkinsfile"
                    '''
                }
            }
        }
    }
}
