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
                }
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                  sh "mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=devsecopsapp  -Dsonar.host.url=http://18.116.39.123:9000/  -Dsonar.login=${SONAR_TOKEN} -Dsonar.projectName=devsecopsapp"
                }
                sh "mvn -Dmaven.test.failure.ignore=true -s settings.xml clean deploy"
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
