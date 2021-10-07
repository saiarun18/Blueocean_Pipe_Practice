pipeline {
  agent any
  stages {
    stage('Dev Code Pull') {
      steps {
        git 'https://github.com/TestLeafInc/WebApp.git'
      }
    }

    stage('Dev Maven Build') {
      steps {
        bat 'start /min StopApp.bat'
        bat 'mvn install'
        bat 'set JENKINS_NODE_COOKIE=dontKillMe && start /min StartApp.bat'
      }
    }

    stage('QA Deploy') {
      steps {
        echo 'Build Deployed in QA Environment'
      }
    }

    stage('QA Testing - Web UI') {
      parallel {
        stage('QA Testing - Web UI') {
          steps {
            git 'https://github.com/TestLeafInc/WebAppUiAutomation.git'
            sleep(time: 10, unit: 'SECONDS')
            bat 'mvn test'
            ws(dir: 'workspace/WebAppUIAutomation')
          }
        }

        stage('API Testing') {
          steps {
            git 'https://github.com/TestLeafInc/WebAppApiAutomation.git'
            sleep(time: 10, unit: 'SECONDS')
            bat 'mvn test'
            ws(dir: 'workspace/WebAppUIAutomation')
          }
        }

      }
    }

  }
}