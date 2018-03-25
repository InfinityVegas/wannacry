node {
   
   stage('SCM Checkout') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/InfinityVegas/wannacry.git'
     
   }
   stage('Maven Build') {
    echo 'Build is started'
      withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.5.3') {
         sh 'mvn clean compile  '
      }
   }
   stage('Test Execution') {
    echo 'Test is executed' 
      withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.5.3') {
         sh 'mvn test'
      }
   }
   
   stage('SonarQube Analysis') {
   
      //def job = build job: 'SonarQubeJob'
      withSonarQubeEnv("Sonarcloud") {  // SonarCloud is Configure system properties set under Manage Jenkins
         withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.5.3') {
           sh 'mvn package sonar:sonar'  
          //'-f /pom.xml ' +
          //'-Dsonar.projectKey=WannaCry:WannaCry ' +
          //'-Dsonar.login=manee2k6 ' +
          //'-Dsonar.password=9c4ba1e2ae9234a20a7f83a726589c229fcac5e2 ' +
          //'-Dsonar.language=java ' +
          //'-Dsonar.sources=. ' +
          //'-Dsonar.tests=. ' +
          //'-Dsonar.test.inclusions=**/*Test*/** ' +
          //'-Dsonar.exclusions=**/*Test*/**' 
         }
      }
   }
   
   stage("Quality Gate") {
      timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
      def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
       if (qg.status != 'OK') {
            error "Pipeline aborted due to quality gate failure: ${qg.status}"
         }
      }
    }
   
   
   stage('Archival') {
    echo 'archived to JFrog repo' 
   }
   stage('Deploy to Dev') {
    echo 'Deploy to Dev' 
   }
}
