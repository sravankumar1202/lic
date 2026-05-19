
def registry = 'https://trialymf8z2.jfrog.io/'
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
    stage('War Publish') {
            steps {
                script {
                    echo "---------------- War Publish Started ----------------"
                    
                    def server = Artifactory.newServer url: registry + '/artifactory', credentialsId: 'jfrog'
                    def properties = "buildid=${env.BUILD_ID}, commitid=${GIT_COMMIT}"
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "target/*.war",
                                "target": "lic-libs-snapshot-local/{1}/",
                                "flat": "false",
                                "props": "${properties}",
				"exclusions": [ "*.sha1", "*.md5"]
			    }
			]
                    }"""
                    def buildInfo = server.upload(uploadSpec)
                    buildInfo.env.collect()
                    server.publishBuildInfo(buildInfo)
                    echo "---------------- War Publish Ended ----------------"
                }
            }
        }
    }
}
