node {
   // Mark the code checkout 'stage'....
   stage 'Checkout'

   // Get some code from a GitHub repository
 // git url: 'https://github.com/jglick/simple-maven-project-with-tests.git'
checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/sarjunkumar24391/Spring-PetClinic.git']]]

   // Get the maven tool.
   // ** NOTE: This 'M3' maven tool must be configured
   // **       in the global configuration.           
   def mvnHome = tool 'M3'

   // Mark the code build 'stage'....
   stage 'Build'
   // Run the maven build
   bat "${mvnHome}/bin/mvn -Dmaven.test.failure.ignore clean package"
   //step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])

    stage 'Analysis'
   // bat 'dir'
bat 'mvn sonar-runner -v'
    // bat 'sonar-runner'
    bat 'mvn sonar:sonar'
    parallel (
    "stream 1" : { 
                     node {
                           bat "echo hstream1"
                       } 
                   }
    )
    stage 'Deploy'
    bat 'pscp target/*.war http://localhost:8089/mywebapp/'
    
}
