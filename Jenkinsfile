pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        S3_BUCKET = 'hello-my-static-site-bucket'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Manju-dotcom871/my-static-site.git', branch: 'main'
            }
        }
        stage('Deploy to S3') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-s3-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        aws s3 sync . s3://$S3_BUCKET --delete --exclude ".git/*" --exclude "Jenkinsfile"
                    '''
                }
            }
        }
    }
}
