pipeline {
    agent any
    stages {
        stage('Git Pull') {
            steps {
                git 'https://github.com/WebGoat/WebGoat'
            }
        }
        stage('OWASP Deps Checker') {
            steps {
                dependency-Check additionalArguments: '', odcInstallation: 'Default'
            }
        }
        stage('OWASP Report') {
            steps {
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
        stage('Sonarqube'){
            steps{
                withSonarQubeEnv('LocalSonar') {
                     bat "${tool("SonarQubeScanner")}/bin/sonar-scanner \
                     -Dsonar.projectKey=webgoat-proj \
                     -Dsonar.sources=.\
                     -Dsonar.exclusions=**/*.java"
                }
            }
        }
	stage('Build'){
	    steps{
		echo 'Done'
	    }
	}
    }
}
