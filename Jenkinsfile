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
    }
}
