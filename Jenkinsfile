pipeline{

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('fatima')
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t tome19290/runway:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push tome19290/runway:latest'
			}
		}
	}

	post {
		always {
			sh 'docker logout'
		}
	}

}