pipeline {
    agent any
    environment {
        PATH ="/opt/maven/bin:$PATH"
    }
    stages{
        stage('biuld'){
            steps{
                sh 'mvn clean install'
            }
        }
   stage ('sonarqube analysis') {
    environment{
        scannerHome = tool 'sonarqube-scanner'
    }
    steps {
        withSonarQubeEnv('sonarqube-server'){
            
            sh "${scannerHome}/bin/sonar-scanner"
        }
    }
}
    }
}
