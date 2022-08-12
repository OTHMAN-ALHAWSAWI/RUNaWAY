pipeline {
    agent any

    environment {

        AWS_ACCESS_KEY_ID     = credentials('othman-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('othman-aws-secret-access-key')
        DOCKERHUB_CREDENTIALS = credentials('OthmanALhawsawei-dockerhub-token')

        AWS_S3_BUCKET         = "othmanalhawsawi-belt2-artifacts-123456"

        ARTIFACT_NAME         = "Dockerrun.aws.json"
        AWS_EB_APP_NAME       = "othman-docker"
        AWS_EB_APP_VERSION    = "${BUILD_ID}"
        AWS_EB_ENVIRONMENT    = "Othmandocker-env"

    }

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t mano10h/runaway:latest .'
            }
        }

        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push') {
            steps {
                sh 'docker push mano10h/runaway:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh 'aws configure set region us-east-1    '
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT_NAME --version-label $AWS_EB_APP_VERSION'
            }
    }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
    
