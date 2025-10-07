pipeline {
    agent any

    environment {
        S3_BUCKET = 'jenkins-maven-deploy-demo'
        REGION = 'ap-south-1'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/Savitha-savi1/simple-project1.git', branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                // Make sure Maven is installed and configured in Jenkins
                withMaven(maven: 'Maven 3') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Deploy to S3') {
            steps {
                // Use the username/password credentials (AWS Access Key & Secret Key)
                withCredentials([usernamePassword(credentialsId: 'aws-creds', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                        echo "Deploying WAR to S3..."
                        aws configure set aws_access_key_id $AWS_ACCESS_KEY_ID
                        aws configure set aws_secret_access_key $AWS_SECRET_ACCESS_KEY
                        aws configure set default.region $REGION
                        aws s3 cp target/*.war s3://$S3_BUCKET/
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Deployment successful!'
        }
        failure {
            echo 'Build or Deployment failed!'
        }
    }
}
