node {
    // reference to maven
    // ** NOTE: This 'maven-3.6.1' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'maven'

    // holds reference to docker image
    def dockerImage
    // ip address of the docker private repository(nexus)
    
    def dockerRepoUrl = "trapthyshetty"
    def dockerImageName = "insead-demo"
    def dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"
    
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/trapthy/codetest-springboot.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.6.1' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven'
    }    
  
    stage('Build Project') {
      // build project via maven
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
    }
	
// 	stage('Publish Tests Results'){
//       parallel(
//         publishJunitTestsResultsToJenkins: {
//           echo "Publish junit Tests Results"
// 		  junit '**/target/surefire-reports/TEST-*.xml'
// 		  archive 'target/*.jar'
//         },
//         publishJunitTestsResultsToSonar: {
//           echo "This is branch b"
//       })
//     }
		
    stage('Build Docker Image') {
      // build docker image
      sh "whoami"
   //   sh "ls -all /var/run/docker.sock"
      sh " docker ps"
   //   sh "mv ./target/insead*.jar ./data" 
      
      dockerImage = docker.build("insead-demo")
    }
   
    stage('Deploy Docker Image'){
      
      // deploy docker image to nexus

      echo "Docker Image Tag Name: ${dockerImageTag}"

     // sh "docker login -u trapthyshetty -p admin123 ${dockerRepoUrl}"
      sh "docker tag ${dockerImageName} ${dockerImageTag}"
      sh "docker push ${dockerImageTag}"
    }
}
