pipeline {
    agent { label  'maven-label' }

    tools {
        maven "M3"
    }

    stages {
        stage('Source') {
            steps {             
                checkout scm
            }
        }
        stage('SonarScanner'){
            steps {
             withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                  sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=devsecopsapp  -Dsonar.host.url=http://172.31.16.115:9000/  -Dsonar.token=${SONAR_TOKEN} -Dsonar.projectName=devsecopsapp"
                }
            }
        }
        stage('buildandupload'){
            steps {
                withCredentials([string(credentialsId: 'NEXUS_PASSWORD', variable: 'NEXUS_PASSWORD')]) {
                sh 'sed  -i "s/passvar/$NEXUS_PASSWORD/g" settings.xml'   
                sh "mvn -Dmaven.test.failure.ignore=true -s settings.xml clean package"
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
