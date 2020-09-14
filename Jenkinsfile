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
//          sh '"$MVN_HOME/bin/mvn" -Dspotbugs.failOnError=false install'
         sh '"$MVN_HOME/bin/mvn" --show-version --batch-mode --errors --no-transfer-progress -Dmaven.test.failure.ignore=true -Dspotbugs.failOnError=false  check verify com.github.spotbugs:spotbugs-maven-plugin.4.0.4:spotbugs'
//         sh '"$MVN_HOME/bin/mvn" --show-version --batch-mode --errors --no-transfer-progress -Dmaven.test.failure.ignore=true -Dspotbugs.failOnError=false  clean verify install'
      }
   }
   stage('Results') {
       archiveArtifacts 'target/*.jar'
       junit '**/target/surefire-reports/TEST-*.xml'
       recordIssues(tool: spotBugs(), qualityGates: [[threshold: 1, type: 'TOTAL', unstable: true]])
   }
}
