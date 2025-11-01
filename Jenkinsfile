node{

	echo "The Build Number is: ${env.BUILD_NUMBER}"
	echo "The Node Name is: ${env.NODE_NAME}"
	echo "The Job Name is: ${env.JOB_NAME}"
	echo "The Jenkins Home is: ${env.JENKINS_HOME}"
	
    def mavenHome = tool name: 'maven3.9.10'
	
    stage('CheckOutCode'){
        git branch: 'development', credentialsId: 'a182db10-3dce-4ff8-957e-afedf9723109', 
         url: 'https://github.com/mithuntechno2025/maven-web-application.git'
    }
	
	stage('CreatWar'){
        sh "${mavenHome}/bin/mvn clean package"
    }

    stage('ExecuteSonarQubeReport'){
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }
	
	stage('UploadArtifactIntoNexus'){
        sh "${mavenHome}/bin/mvn clean deploy"
    }
	
    stage('DeployOnTomcat'){
        sshagent(['dd5149fc-bf81-472d-b21e-ab9459f8cbef']) {
		   sh "chmod -R 777 /opt/web-server/apache-tomcat-9.0.109/webapps/"
           sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.5.233:/opt/web-server/apache-tomcat-9.0.109/webapps/"
      }
    }
 }  
