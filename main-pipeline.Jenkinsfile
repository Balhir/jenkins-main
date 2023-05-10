pipeline {
	
	agent any

	stages {
	    
	    stage('Application code checkout') {
			steps {
				git branch: 'main',
				url: 'https://github.com/Balhir/jib-hello-world.git'
			}
		}
		
		stage('Application build and image create') {
			steps {
			    sh 'whoami'
				sh './mvnw compile jib:dockerBuild -Dimage=jib-hello-world:v1'
			}
		}
		
		stage ('Image push to Artifact Repository') {
		    steps {
		        withCredentials([file(credentialsId: 'jenkins-sa', variable: 'secfile')]) {
		        sh "gcloud auth activate-service-account jenkins-sa@proxy-test-svpc.iam.gserviceaccount.com --key-file=$secfile"
		        sh 'gcloud auth configure-docker europe-west2-docker.pkg.dev'
		        sh 'docker tag jib-hello-world:v1 europe-west2-docker.pkg.dev/proxy-test-proj1/gke-images/jib-hello-world:v1'
		        sh 'docker push europe-west2-docker.pkg.dev/proxy-test-proj1/gke-images/jib-hello-world:v1'
		        }
		    }
		}
		
		stage('Kubernetes code checkout') {
		    steps {
		        git branch: 'master',
		        url: 'https://github.com/Balhir/jib-kubernetes.git'
		    }
		}
		
	    
		stage('Kubectl apply') {
			steps {
				withCredentials([file(credentialsId: 'jenkins-sa', variable: 'secfile')]) {
			        sh "gcloud auth activate-service-account jenkins-sa@proxy-test-svpc.iam.gserviceaccount.com --key-file=$secfile"
			        sh "gcloud config set auth/impersonate_service_account jenkins-sa@proxy-test-svpc.iam.gserviceaccount.com"
			        sh "gcloud container clusters get-credentials cluster-1 --region europe-west2 --project proxy-test-proj1"
			        sh "kubectl apply -f hello-app.deployment.yaml"
			        sh "kubectl rollout status deployments/hello-app"
			    }
			}
		}
	}
}