node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/justnoxx/java-maven-junit-helloworld.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'mvn'
   }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
          sh '"$MVN_HOME/bin/mvn" install'
      }
   }
   stage('Results') {
       archiveArtifacts 'target/*.jar'
               script {
                       def testResults = findFiles(glob: '**/target/surefire-reports/TEST-*.xml')
                       for(xml in testResults) {
                           touch xml.getPath()
                       }
                   }
               junit '**/target/surefire-reports/TEST-*.xml'
               recordIssues(tool: spotBugs(), qualityGates: [[threshold: 1, type: 'TOTAL', unstable: true]])
   }
}
