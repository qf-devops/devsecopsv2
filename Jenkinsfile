pipeline {
    agent any

    tools {
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
               
                checkout scm

                withCredentials([string(credentialsId: 'NEXUS_PASSWORD', variable: 'NEXUS_PASSWORD')]) {
                sh 'sed  -i "s/passvar/\\"$NEXUS_PASSWORD\\"/g" settings.xml'
                sh "cat settings.xml"
                sh "mvn -Dmaven.test.failure.ignore=true -s settings.xml clean deploy"
                }

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
