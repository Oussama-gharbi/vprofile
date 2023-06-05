pipeline {

    agent any
/*
	tools {
        maven "maven3"
    }
*/
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "10.165.147.221:8081"
        NEXUS_REPOSITORY = "vprofile-release"
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        ARTVERSION = "${env.BUILD_ID}"
        NEXUS_REPOGRP_ID    = "vprofile-maven-group"
        scannerHome= tool 'mysonarscanner4'
    }

    stages{

        // stage('Fetch Code') {
        //     steps {
        //         git branch: 'main', url: 'https://github.com/Oussama-gharbi/vprofile.git'
        //     }
        // }
        // stage('BUILD'){
        //     steps {
        //         sh 'mvn clean install -DskipTests'
        //     }
        //     post {
        //         success {
        //             echo 'Now Archiving...'
        //             archiveArtifacts artifacts: '**/target/*.war'
        //         }
        //     }
        // }

        // stage('UNIT TEST'){
        //     steps {
        //         sh 'mvn test'
        //     }
        // }

        // stage('INTEGRATION TEST'){
        //     steps {
        //         sh 'mvn verify -DskipUnitTests'
        //     }
        // }

        // stage ('CODE ANALYSIS WITH CHECKSTYLE'){
        //     steps {
        //         sh 'mvn checkstyle:checkstyle'
        //     }
        //     post {
        //         success {
        //             echo 'Generated Analysis Result'
        //         }
        //     }
        // }

        // stage('CODE ANALYSIS with SONARQUBE') {

        //     environment {
        //         scannerHome = tool 'mysonarscanner4'
        //     }

        //     steps {
        //         withSonarQubeEnv('sonar-pro') {
        //             sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
        //            -Dsonar.projectName=vprofile-repo \
        //            -Dsonar.projectVersion=1.0 \
        //            -Dsonar.sources=src/ \
        //            -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
        //            -Dsonar.junit.reportsPath=target/surefire-reports/ \
        //            -Dsonar.jacoco.reportsPath=target/jacoco.exec \
        //            -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
        //         }

        //         // timeout(time: 10, unit: 'MINUTES') {
        //         //     waitForQualityGate abortPipeline: true
        //         // }
        //     }
        // }

    //     stage("Publish to Nexus Repository Manager") {
    //         steps {
    //             nexusArtifactUploader(
    //     nexusVersion: "${NEXUS_VERSION}",
    //     protocol: "${NEXUS_PROTOCOL}",
    //     nexusUrl: "${NEXUS_URL}",
    //     groupId: 'com.example',
    //     version: "${env.BUILD_ID}",
    //     repository: "${NEXUS_REPOSITORY}",
    //     credentialsId: "${NEXUS_CREDENTIAL_ID}",
    //     artifacts: [
    //         [artifactId: 'vprofile',
    //          classifier: '',
    //          file: 'target/vprofile-v2.war',
    //          type: 'war']
    //     ]
    //  )
    //             }
    //         }
        
	    stage('deploy to Tomcat') {
     steps {
                script {
                   echo 'deploying artifact  to Tomcat...'

                   //def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                   def ec2Instance = "ec2-user@35.180.251.121"

                   sshagent(['tomcat-server-key']) {
                       sh "ssh -o StrictHostKeyChecking=no ubuntu@10.165.147.248"
                       sh "curl -u admin:nexus -o artifact.war 'http://10.165.147.221:8081/repository/vprofile-maven-group/com/example/vprofile/*.war'"
                       //sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                       //sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                        // ssh user1@server1 command1
                        //   ssh user1@server1 'command2'
                        //   # pipe #
                        //  ssh user1@server1 'command1 | command2'
                        //    # multiple commands (must enclose in quotes #
                        //   ssh admin@box1 "command1; command2; command3"
                   }

    }


}
    }
}
}
