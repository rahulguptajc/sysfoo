pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'building sysfoo app...'
        sh 'mvn compile'
      }
    }

    stage('unit tests') {
      agent {
        docker {
          image 'maven:3.6.3-jdk-11-slim'
        }

      }
      steps {
        echo 'running unit tests'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      parallel {
        stage('package') {
          agent {
            docker {
              image 'maven:3.6.3-jdk-11-slim'
            }

          }
          when {
            branch 'master'
          }
          steps {
            echo 'generating .war file'
            sh 'mvn package -DskipTests'
            archiveArtifacts '**/target/*.war'
          }
        }

        stage('Docker B&P') {
          agent any
          when {
            branch 'master'
          }
          steps {
            script {
              docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                def dockerImage = docker.build("initcron/sysfoo:v${env.BUILD_ID}", "./")
                dockerImage.push()
                dockerImage.push("latest")
                dockerImage.push("dev")
              }
            }

          }
        }

      }
    }

    stage('Deploy to Dev') {
      agent any
      when {
            branch 'master'
      }
      steps {
        sh 'docker-compose up -d '
      }
    }

  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}
