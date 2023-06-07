pipeline {

    agent any


    stages{

         stage('Fetch Code') {
             steps {
                 git branch: 'main', url: 'https://github.com/Oussama-gharbi/vprofile.git'
             }
	 }
                  
	    stage('deploy to Tomcat') {
     steps {
                script {
                      sh "curl -u admin:nexus -o newartifact 'http://10.165.147.221:8081/repository/vprofile-release/com/example/vprofile/22/vprofile-22.war'"
                   sshagent(credentials: ['tomcat-server-key']) {
			   sh "scp -o StrictHostKeyChecking=no ubuntu@10.165.147.248:/usr/local/tomcat8/webapps/vprofile" 
			  }
    }


}
    }
}
}
