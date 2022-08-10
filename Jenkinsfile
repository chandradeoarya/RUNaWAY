pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('jenkins')
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t adwaa/dojo-jump:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push adwaa/dojo-jump:latest'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}
