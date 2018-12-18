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
				sh 'docker run --rm -vci_jenkins:/var/jenkins_home -v/var/run/docker.sock:/var/run/docker.sock -vm2:/root/.m2 -w"$PWD" --net=ci_default maven:3.5.2-jdk-8-alpine mvn --settings settings.xml deploy'
				sh 'docker push nexus:8082/treeptik/helloworld'
			}
		}
		stage('Deploy QA') {
			steps {
				sh 'docker stop ci_QA || true && docker rm -f ci_QA || true'
				sh 'docker run -d -p 8380:8080 --name ci_QA nexus:8082/treeptik/helloworld:latest'
			}
		}
		stage('Integration Tests') {
			steps {
				sh 'sleep 10'
			}
		}
		stage('Deploy PrePROD') {
			steps {
				sh 'docker stop ci_PrePROD || true && docker rm -f ci_PrePROD || true'
				sh 'docker run -d -p 8381:8080 --name ci_PrePROD nexus:8082/treeptik/helloworld:latest'
			}
		}
		stage('PrePROD Tests') {
			steps {
				sh 'sleep 10'
			}
		}
		stage('Deploy PROD') {
			steps {
				sh 'docker stop ci_PROD || true && docker rm -f ci_PROD || true'
				sh 'docker run -d -p 8382:8080 --name ci_PROD nexus:8082/treeptik/helloworld:latest'
			}
		}
	}
}
