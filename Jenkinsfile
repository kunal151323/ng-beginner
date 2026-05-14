pipeline {
    agent any

    environment {
        S3_BUCKET = 'myk5.com'
        AWS_DEFAULT_REGION = 'ap-south-1'
    }

    stages {

        stage('Check Node') {
            steps {
                sh '''
                node -v
                npm -v
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Verify Build Files') {
            steps {
                sh 'find dist'
            }
        }

        stage('Deploy to S3') {
            steps {

                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: '62388c0f-5d6c-414a-b1a7-c3b121766fae']
                ]) {

                    sh '''
                    aws s3 sync dist/ng-beginner/ s3://$S3_BUCKET --delete
                    '''
                }
            }
        }
    }
}
