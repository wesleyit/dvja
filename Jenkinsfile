pipeline {
  agent any

  tools {
    maven "apache-maven-3.6.3"
  }

  stages {
    stage('Build') {
      steps {
        git 'https://github.com/ajlanghorn/dvja.git'
        sh "mvn clean package"
      }
    }
    stage('Check dependencies') {
      steps {
        dependencyCheck additionalArguments: '', odcInstallation: 'Dependency-Check'
        dependencyCheckPublisher failedTotalCritical: 1000, failedTotalHigh: 1000, failedTotalLow: 1000, failedTotalMedium: 1000, pattern: '', unstableTotalCritical: 500, unstableTotalHigh: 500, unstableTotalLow: 500, unstableTotalMedium: 500
      }
    }
    stage('Publish to S3') {
        steps {
        sh "aws s3 cp /var/lib/jenkins/workspace/dvja/target/dvja-1.0-SNAPSHOT.war s3://ako2020-buildartifacts-1lse0u0q3dyzk/dvja-1.0-SNAPSHOT.war"
      }
    }
    stage('Tidy up') {
      steps {
        cleanWs()
      }
    }
  }
}
