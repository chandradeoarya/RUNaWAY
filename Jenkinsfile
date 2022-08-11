pipeline {

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('fatima')
		AWS_ACCESS_KEY_ID     = credentials('fatima-aws-secret-key-id')
  		AWS_SECRET_ACCESS_KEY = credentials('fatima-aws-secret-access-key')
		ARTIFACT_NAME = 'Docker.aws.json'
		AWS_S3_BUCKET = 'fatima-belt2-artifacts-12345677'
		AWS_EB_APP_NAME = 'docker-'
        AWS_EB_ENVIRONMENT_NAME = 'docker-env'
        AWS_EB_APP_VERSION = "${BUILD_ID}"
        DOCKERHUB_IMG = 'tome19290/runway'
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t $DOCKERHUB_IMG .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push $DOCKERHUB_IMG'
			}
		}

        stage('Deploy') {
            steps {
                sh 'aws configure set region us-east-1	'
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT_NAME --version-label $AWS_EB_APP_VERSION'
            }
	}
    
	stage ("Logout") {
		steps {
			sh 'docker logout'
		    }
    	}
    }
}