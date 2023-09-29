pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'building sysfoo app...'
        sh 'mvn compile'
      }
    }

    stage('test') {
      parallel {
        stage('unit tests') {
          steps {
            echo 'running unit tests'
            sh 'mvn clean test'
          }
        }

        stage('integration tests') {
          steps {
            echo 'mock stage'
            sleep 5
          }
        }

        stage('SCA') {
          steps {
            sleep 8
          }
        }

      }
    }

    stage('package') {
      parallel {
        stage('package') {
          steps {
            echo 'generating .war file'
            sh 'mvn package -DskipTests'
            archiveArtifacts '**/target/*.war'
          }
        }

        stage('pkg2') {
          steps {
            sleep 6
          }
        }

      }
    }

  }
  tools {
    maven 'Maven 3.6.3'
  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}
