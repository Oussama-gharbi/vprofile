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
                     
                   sshagent(['tomcat-server-key']) {
                       	sh "ssh -o StrictHostKeyChecking=no ubuntu@10.165.147.248 pwd " 
			sh 'pwd' 
                       }
			sh 'pwd'
    }


}
    }
}
}
