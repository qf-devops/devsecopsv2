pipeline {
    agent { label  'maven-label' }

    tools {
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
               
                checkout scm

                withCredentials([string(credentialsId: 'NEXUS_PASSWORD', variable: 'NEXUS_PASSWORD')]) {
                sh 'sed  -i "s/passvar/$NEXUS_PASSWORD/g" settings.xml'
                withSonarQubeEnv() {
                  sh "mvn clean verify sonar:sonar -Dsonar.projectKey=devsecopsapp -Dsonar.projectName='devsecopsapp'"
                }
                sh "mvn -Dmaven.test.failure.ignore=true -s settings.xml clean deploy"

                }
            }

            post {

                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
