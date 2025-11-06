pipeline {
  agent any

  tools {
    jdk 'JDK21'       // name you configured
    maven 'Maven3'    // name you configured
  }

  environment {
    MAVEN_OPTS = "-Xmx512m"
  }

  stages {
    stage('Checkout') {
      steps {
       
        checkout scm
      }
    }

    stage('Build') {
      steps {
        script {
          if (isUnix()) {
            sh "mvn -B -DskipTests=true clean package"
          } else {
            bat 'mvn -B -DskipTests=true clean package'
          }
        }
      }
    }

    stage('Test') {
      steps {
        script {
          if (isUnix()) {
            sh "mvn -B test"
          } else {
            bat "mvn -B test"
          }
        }
      }
      post {
        always {
          // publish JUnit test reports (adjust path if different)
          junit '**/target/surefire-reports/*.xml'
        }
      }
    }

    stage('Deploy') {
      steps {
        script {
          archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
          if (isUnix()) {
            sh 'mkdir -p deploy || true; cp target/*.jar deploy/ || true; echo "Artifact copied to deploy/"'
          } else {
            bat 'if not exist deploy mkdir deploy\ncopy /Y target\\*.jar deploy\\'
          }
        }
      }
    }
  }

  post {
    success {
      echo "Pipeline completed successfully!"
    }
    failure {
      echo "Pipeline failed."
    }
  }
}
