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
      //def job = build job: 'SonarQubeJob'
      withSonarQubeEnv("Sonarcloud") {  // SonarCloud is Configure system properties set under Manage Jenkins
         withMaven(jdk: 'JDK-1.8', maven: 'Maven-3.3.9') {
           sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar' + 
          //'-f /pom.xml ' +
          //'-Dsonar.projectKey=WannaCry:WannaCry ' +
          //'-Dsonar.login=manee2k6 ' +
          //'-Dsonar.password=9c4ba1e2ae9234a20a7f83a726589c229fcac5e2 ' +
          '-Dsonar.language=java ' +
          '-Dsonar.sources=. ' +
          '-Dsonar.tests=. ' +
          '-Dsonar.test.inclusions=**/*Test*/** ' +
          '-Dsonar.exclusions=**/*Test*/**' 
         }
      }
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
