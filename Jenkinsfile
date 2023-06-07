pipeline {

    agent any
environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "10.165.147.221:8081"
        NEXUS_REPOSITORY = "vprofile-release"
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        NEXUS_REPOGRP_ID    = "vprofile-maven-group"
        scannerHome= tool 'mysonarscanner4'
    }

    stages{

         stage('Fetch Code') {
             steps {
                 git branch: 'main', url: 'https://github.com/Oussama-gharbi/vprofile.git'
             }
	 }
	 stage('increment version') {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
		 echo "$version" 
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }  
         stage('BUILD'){
             steps {
                 sh 'mvn clean install -DskipTests'
             }
            post {
                success {
                    echo 'Now Archiving...'
              //      archiveArtifacts artifacts: "**/target/*.war"
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
                scannerHome = tool 'mysonarscanner4'
            }

            steps {
                withSonarQubeEnv('sonar-pro') {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }

                // timeout(time: 10, unit: 'MINUTES') {
                //     waitForQualityGate abortPipeline: true
                // }
            }
        }

        stage("Publish to Nexus Repository Manager") {
            steps {
                nexusArtifactUploader(
        nexusVersion: "${NEXUS_VERSION}",
        protocol: "${NEXUS_PROTOCOL}",
        nexusUrl: "${NEXUS_URL}",
        groupId: 'com.example',
        version: "${env.BUILD_ID}",
        repository: "${NEXUS_REPOSITORY}",
        credentialsId: "${NEXUS_CREDENTIAL_ID}",
        artifacts: [
            [artifactId: 'vprofile',
             classifier: '',
	     file: "target/vprofile-$version.war",
             type: 'war']
        ]
     )
                }
            }         
	//    stage('deploy to Tomcat') {
   //  steps {
     //           script {
      //                sh "curl -u admin:nexus -o newartifact.war 'http://10.165.147.221:8081/repository/vprofile-release/com/example/vprofile/22/vprofile-22.war'"
     //              sshagent(credentials: ['tomcat-server-key']) {
	//		   sh "scp -o StrictHostKeyChecking=no newartifact.war ubuntu@10.165.147.248:/usr/local/tomcat8/webapps/vprofile" 
	//		  }
//}


}
    }

