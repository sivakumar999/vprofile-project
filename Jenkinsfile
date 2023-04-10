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
  stage('UNIT TEST'){
            steps {
                sh 'mvn test'
            }
        }

	stage('INTEGRATION TEST'){
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
		
        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
	 
stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool 'sonarscanner'
          }

          steps {
            withSonarQubeEnv('sonarserver') {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }

            
          }
        }

stage("Quality Gate") {
 steps {
 timeout(time: 2, unit: 'MINUTES') {
 // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate 
fails
 // true = set pipeline to UNSTABLE, false = don't
waitForQualityGate abortPipeline: true
 }
 }
 }


 }
}
