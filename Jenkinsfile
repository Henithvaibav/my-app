pipeline {
    agent any
    //tool name: 'Maven-3', type: 'maven'
    stages {
  
  stage('SCM_Checkout') {
    steps {
        git credentialsId: 'github-private-key', url: 'git@github.com:Henithvaibav/my-app.git'
    }
  }
  stage('Sonar Qube') {
      steps {
          withSonarQubeEnv('sonar-6') {
    sh 'mvn sonar:sonar'
}
      }
  }
        stage('Sonar Qube Status checks') {
        steps {
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if ("${qg.status}" != 'OK') {
                slackSend baseUrl: 'https://hooks.slack.com/services/', 
                channel: 'myapp', 
                color: 'danger', message: 'Mark this build as failure', 
                teamDomain: 'mailtoindrajith', 
                tokenCredentialId: 'slack-myapp', 
                username: 'mailtoindrajith'
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }  
}        
    stage('Maven Build') {
    steps {
      sh 'mvn clean package'
    }
  }
        stage('Slack Notification') {
            steps {
            slackSend baseUrl: 'https://hooks.slack.com/services/', 
                channel: 'myapp', 
                color: 'good', message: 'welcome slack !!!', 
                teamDomain: 'mailtoindrajith', 
                tokenCredentialId: 'slack-myapp', 
                username: 'mailtoindrajith'
            }
        }

   stage('Deploy war file tomcat') {
    steps {
      sh 'cp target/*.war /opt/apache-tomcat-10.0.8/webapps/'
    }
  }

}
}
