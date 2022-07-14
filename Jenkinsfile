pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn clean package' 
            }
  
                    post {
                success {
                    // we only worry about archiving the jar file if the build steps are successful
                    archiveArtifacts(artifacts: '**/target/*.war', allowEmptyArchive: true)
                }
            }
        }      
        stage('test'){
            steps{
                sh 'mvn clean test'
            }
        }
        

        
    }
}
