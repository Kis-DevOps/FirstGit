pipeline{
  agent any
  
  tools{
  maven '3.0'
  jdk 'java 8'
  
  }
    stages{
      stage("initialise"){
	   steps{
	   sh '''
      echo "PATH = ${PATH}"
      echo "M2_HOME = ${M2_HOME}"
      ''' 
	  }
	}
	stage("Git Checkout"){
      steps{
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/kliakos/sparkjava-war-example.git']]])
           }
          }
	 stage("Build Project"){
	  steps{
	   sh 'mvn package -DskipTest' 
	  }
	}
	
	 stage("Artifactory Deploy"){
	   steps{
	    nexusArtifactUploader artifacts: [[artifactId: 'java-hello-world', classifier: '', file: 'target/sparkjava-hello-world-1.0.war', type: 'war']], credentialsId: 'nexus', groupId: 'kishore', nexusUrl: '52.15.155.53:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'java', version: '2.$BUILD_NUMBER'
	  }
	}
	
	 stage("DeployTomcat"){
	  steps{
	   deploy adapters: [tomcat7(credentialsId: 'TomCAT', path: '', url: 'http://3.142.45.243:8080')], contextPath: 'kisho', war: '**/*.war'
	  }
	}
	
  }

}
