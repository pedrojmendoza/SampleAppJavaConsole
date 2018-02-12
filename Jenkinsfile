pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests -f ./my-app/pom.xml clean package'
      }
    }
    stage('Test') {
        steps {
            sh 'mvn -f ./my-app/pom.xml test'
        }
        post {
            always {
                junit './my-app/target/surefire-reports/*.xml'
            }
        }
    }
  }
}
