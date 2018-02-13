pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
      args '-v /root/.m2:/root/.m2:z -u root'
      reuseNode true
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
                junit 'my-app/target/surefire-reports/*.xml'
            }
        }
    }
    stage('Help') {
        steps {
            sh 'ls -la /root/.m2'
            sh 'less /root/.m2/settings.xml'
            sh 'mvn help:effective-settings'
        }
    }
    stage('Deploy') {
        steps {
            sh 'mvn -X deploy:deploy-file -DgroupId=com.mycompany.app -DartifactId=App -Dversion=1.0.0-SNAPSHOT -DgeneratePom=true -Dpackaging=jar -DrepositoryId=nexus -Durl=http://ec2-54-197-45-165.compute-1.amazonaws.com:8081/repository/maven-snapshots -Dfile=./my-app/target/my-app-1.0-SNAPSHOT.jar'
        }
    }
    stage('Run') {
        steps {
            sh 'java -cp my-app/target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App'
        }
    }
  }
}
