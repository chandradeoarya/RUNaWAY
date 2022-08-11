pipeline {

	agent any

	environment {

		// Jenkins global credentials

        AWS_ACCESS_KEY_ID         = credentials('almutaywia-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY     = credentials('almutaywia-aws-secret-access-key')
        DOCKERHUB_PASS_LOGIN      = credentials('almutaywia-dockerhub-token')

        // Docker hub user login

        DOCKERHUB_USR_LOGIN = 'abdullah01455'

		// AWS vars

        S3_ARTIFACT_DOCKERRUN_FILE= 'Dockerrun.json'
	AWS_S3_BUCKET             = 'almutaywia-belt2-artifacts-123456'
	AWS_EB_APP_NAME           = 'beltexam-app-env'
        AWS_EB_ENVIRONMENT_NAME   = 'Beltexamappenv-env'
        AWS_EB_APP_VERSION        = "${BUILD_ID}"
        AWS_REGION                = 'us-east-1'
        
	}

	stages {

		stage('Build') {
			steps {
				sh 'docker build -t abdullah01455/beltexam2_runwaygame:latest .'
			}
		}

		stage('Login') {
			steps {
				sh 'docker login -u=$DOCKERHUB_USR_LOGIN -p=$DOCKERHUB_PASS_LOGIN'
			}
		}

		stage('Push') {
			steps {
				sh 'docker push abdullah01455/beltexam2_runwaygame:latest'
			}
		}

        stage('Deploy') {
            steps {
                sh 'aws configure set region $AWS_REGION'
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$S3_ARTIFACT_DOCKERRUN_FILE'
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
