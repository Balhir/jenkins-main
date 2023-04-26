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
	}
}
