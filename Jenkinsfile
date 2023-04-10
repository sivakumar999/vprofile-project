pipeline {
 agent any
/* tools {
 maven "MAVEN3"
 jdk "OracleJDK8"
 }
 */
 environment {
 SNAP_REPO = 'vprofile-snapshot'
 NEXUS_USER = 'admin'
 NEXUS_PASS = '5555'
 RELEASE_REPO = 'vprofile-release'
 CENTRAL_REPO = 'vpro-maven-central'
 NEXUSIP = '172.31.16.234'
 NEXUSPORT = '8081'
 NEXUS_GRP_REPO = 'vpro-maven-group'
 NEXUS_LOGIN = 'nexuslogin'
	 SONARSERVER = 'sonarserver'
         SONARSCANNER = 'sonarscanner'
 }
 stages {
  stage('BUILD'){
            steps {
                sh 'mvn clean install -DskipTests'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
  stage('Sonar Analysis') {
 environment {
 scannerHome = tool "${SONARSCANNER}"
 }
 steps {
 withSonarQubeEnv("${SONARSERVER}") {
 sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
 -Dsonar.projectName=vprofile \
-Dsonar.projectVersion=1.0 \
-Dsonar.sources=src/ \
-Dsonar.java.binaries=target/testclasses/com/visualpathit/account/controllerTest/ \
 -Dsonar.junit.reportsPath=target/surefire-reports/ \
 -Dsonar.jacoco.reportsPath=target/jacoco.exec \
-Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
 }
 }
 }


 }
}
