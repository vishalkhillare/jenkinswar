# jenkinswar

# script : pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        // Replace with your build command
        sh 'mvn clean package'
      }
    }

    stage('Deploy') {
      steps {
        // Replace with your Tomcat credentials
        withCredentials([
          usernamePassword(
            credentialsId: 'tomcat-creds',
            usernameVariable: 'TOMCAT_USERNAME',
            passwordVariable: 'TOMCAT_PASSWORD'
          )
        ]) {
          // Replace with your Tomcat URL and WAR file location
          sh '''
            curl -u $TOMCAT_USERNAME:$TOMCAT_PASSWORD \
            --upload-file target/*.war \
            http://localhost:8080/manager/text/deploy?path=/myapp&update=true
          '''
        }
      }
    }
  }
}


# 1.The script uses the pipeline block to define a Jenkins pipeline.

# 2.The agent block specifies that the pipeline should run on any available Jenkins agent.

# 3.The stages block defines two stages: Build and Deploy.

# 4.The Build stage runs the build command to package the WAR file. Replace the sh command with your build command.

# 5.The Deploy stage deploys the WAR file on Tomcat. 

# 6.The withCredentials block specifies the Tomcat credentials to use for authentication. 

# 7.Replace the credentialsId value with the ID of your Tomcat credentials in Jenkins. 

# 8.The usernameVariable and passwordVariable values specify the environment variables to use for the Tomcat username and password. 

# 9.The sh command uses curl to upload the WAR file to Tomcat using the Tomcat manager API. 

# 10.Replace the Tomcat URL with your Tomcat server URL, and replace the WAR file location with the location of your WAR file.



# Note: that this script assumes that you have already set up Tomcat and Jenkins with the necessary configuration, such as creating a Tomcat user with the appropriate permissions and installing the necessary plugins in Jenkins.
