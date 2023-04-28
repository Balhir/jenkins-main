pipeline {
	
	agent any

	stages {
		stage('Git checkout') {
			steps {
				git branch: 'main',
				url: 'https://github.com/Balhir/jib-hello-world.git'
			}
		}
		stage('Application build') {
			steps {
				sh './mvnw compile jib:dockerBuild -Dimage=jib-hello-world:v1'
			}
		}
		stage('Deploy prepare') {
			steps {
				withCredentials([file(credentialsId: 'jenkins-sa', variable: 'secfile')]) {
			        sh "gcloud auth activate-service-account jenkins-sa@proxy-test-svpc.iam.gserviceaccount.com --key-file=$secfile"
					sh "kubectl get pods"
			    }
			}
		}
	}
}
