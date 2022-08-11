pipeline {

	agent any

	environment {
		DOCKERHUB_CREDENTIALS_PSW = credentials('fatima')
        DOCKERHUB_CREDENTIALS_USR = 'tome19290'
		AWS_ACCESS_KEY_ID     = credentials('fatima-aws-secret-key-id')
  		AWS_SECRET_ACCESS_KEY = credentials('fatima-aws-secret-access-key')
		ARTIFACT_NAME = 'Dockerrun.aws.json'
		AWS_S3_BUCKET = 'fatima-belt2-artifacts-12345677'
		AWS_EB_APP_NAME = 'docker'
        AWS_EB_ENVIRONMENT_NAME = 'docker-env'
        AWS_EB_APP_VERSION = "${BUILD_ID}"
        AWS_REGION = 'us-east-1'
	}

	stages {

		stage('Build') {
			steps {
				sh 'docker build -t fatima/runway:latest .'
			}
		}

		stage('Login') {
			steps {
				sh 'docker login -u=$DOCKERHUB_CREDENTIALS_USR -p=$DOCKERHUB_CREDENTIALS_PSW'
			}
		}

		stage('Push') {
			steps {
				sh 'docker push fatima/runway:latest'
			}
		}

        stage('Deploy') {
            steps {
                sh 'aws configure set region $AWS_REGION'
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