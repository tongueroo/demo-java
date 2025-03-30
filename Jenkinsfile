node{
	echo "Jenkins job name is  : ${env.JOB_NAME}"
	echo "build number is  : ${env.BUILD_NUMBER}"
	echo "node name is  :${env.NODE_NAME}"
    	def mavenHome = tool name : "maven-3.9.9"
        try{
		stage('git-checkot')
        	{
			notifyBuild('STARTED')	
        		git branch: 'feature-1', credentialsId: '6e1718d8-aa75-4745-b8cd-afa034bab3d1', 
        		url: 'https://github.com/Idea-Better-5g/demo-java.git'    
        	}
        	stage('complie'){
            		sh "${mavenHome}/bin/mvn compile"
        	}
        	stage('build'){
           		sh "${mavenHome}/bin/mvn clean package"
        	}
	   	stage('SQ Report')
		{
    			sh  "${mavenHome}/bin/mvn sonar:sonar"
		}
		stage('Deploy into Nexus')
		{
    			sh  "${mavenHome}/bin/mvn clean deploy"
		}
		stage('deploy into tomcat'){
			sshagent(['247df6d4-99a1-46ce-9a95-47649bb3f49a']) {

			sh "scp -o StrictHostKeyChecking=no target/demo.war ec2-user@13.201.4.156:/opt/apache-tomcat-9.0.102/webapps"
			}
		}
	}//try close
	catch(e)   {
        	currentBuild.result="FAILED"
        	throw e
 	   }//catch close
    	finally{
        	notifyBuild(currentBuild.result)
    }//finally close
}//node close
def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }
      def channels = ["#black-tiger", "development", "#dreams-feature","#loginfeatures"]
    // Send Slack message to each channel
    for (channel in channels) {
        slackSend(color: colorCode, message: summary, channel: channel)
    }
}
