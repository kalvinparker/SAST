pipeline {
    agent any
    stages {
        stage('Git Pull') {
      steps {
        sh label: 'Checkout WebGoat', script: '''
          git config remote.origin.url https://github.com/WebGoat/WebGoat
          git fetch --tags --force --progress -- https://github.com/WebGoat/WebGoat +refs/heads/*:refs/remotes/origin/*
          git checkout -f origin/master
        '''
            }
        }
        stage('OWASP Deps Checker') {
            steps {
                dependencyCheck additionalArguments: '', odcInstallation: 'Default'
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
