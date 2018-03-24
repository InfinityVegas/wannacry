node {
   
   stage('SCM Checkout') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/InfinityVegas/wannacry.git'
     
   }
   stage('Maven Build') {
    echo 'Build is started'
      withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.3.9') {
         sh 'mvn clean compile '
      }
   }
   stage('Test Execution') {
    echo 'Test is executed' 
      withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.3.9') {
         sh 'mvn test '
      }
   }
   
   stage('SonarQube Analysis') {
    echo 'Code inspection started..' 
      def job = build job: 'SonarQubeJob'
   }
   
   stage('Package') {
    echo 'Packaged application' 
      withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.3.9') {
         sh 'mvn package '
      }
   }
   stage('Archival') {
    echo 'archived to JFrog repo' 
   }
   stage('Deploy to Dev') {
    echo 'Deploy to Dev' 
   }
}
