node('master') {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git url: 'https://github.com/narendrak144/test.git', branch: 'master'
      def v = version()
      if(v){
          echo 'Building version "${v}"'
      }
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M3'
   }
   stage('Build') {
       input 'ready to go?'
      // Run the maven build
      if (isUnix()) {
          echo 'this is unix system'
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
          echo 'this is windows system'
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
   }
}

def version() {
  def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
  matcher ? matcher[0][1] : null
}
