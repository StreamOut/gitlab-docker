pipeline {
	agent any
	triggers { pollSCM('*/1 * * * *') }

	stages {
		stage('checkout') {
			steps {
				checkout scm
			}
		}
		stage('build & publish') {
			steps {
				sh 'docker run --rm -vroot_jenkins:/var/jenkins_home -v/var/run/docker.sock:/var/run/docker.sock -vm2:/root/.m2 -w"$PWD" maven:3.5.2-jdk-8-alpine mvn --settings settings.xml deploy'
				sh 'docker push nexus:8082/treeptik/helloworld'
			}
		}
		stage('Deploy QA') {
			steps {
				sh 'docker stop QA || true && docker rm -f QA || true'
				sh 'docker run -d -p 8380:8080 --name QA nexus:8082/treeptik/helloworld:latest'
			}
		}
		stage('Integration Tests') {
			steps {
				sh 'sleep 10'
			}
		}
		stage('Deploy PrePROD') {
			steps {
				sh 'docker stop PrePROD || true && docker rm -f PrePROD || true'
				sh 'docker run -d -p 8381:8080 --name PrePROD nexus:8082/treeptik/helloworld:latest'
			}
		}
		stage('PrePROD Tests') {
			steps {
				sh 'sleep 10'
			}
		}
		stage('Deploy PROD') {
			steps {
				sh 'docker stop PROD || true && docker rm -f PROD || true'
				sh 'docker run -d -p 8382:8080 --name PROD nexus:8082/treeptik/helloworld:latest'
			}
		}
	}
}
