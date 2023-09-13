pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn -f ../firstapp/pom.xml clean package'
      }
    }
stage('Unit and Integration Tests') {
  steps {
    sh 'mvn -f ../firstapp/pom.xml test'

    sh 'mvn -f ../firstapp/pom.xml integration-test'
    
  }
  post {
                success {
                    script {
                        def emailBody = "Unit and Integration Tests has passed.Build Status: ${currentBuild.currentResult}"
                        emailext body: emailBody, subject: "Tests Passed", to: "wobushidahundan@gmail.com"
                        emailext attachLog: true, subject: "Tests Scan passed Log", to: "wobushidahundan@gmail.com"
                    }
                }
                failure {
                    script {
                        def emailBody = "Unit and Integration Tests has failed. Please check the logs for more details.Build Status: ${currentBuild.currentResult}"
                        emailext body: emailBody, subject: "Tests Scan Failed", to: "wobushidahundan@gmail.com"
                        emailext attachLog: true, subject: "Tests Scan Failed Log", to: "wobushidahundan@gmail.com"
                    }
                }
            }
  
}
    stage('Code Analysis') {
      steps {
        sh 'mvn -f ../firstapp/pom.xml findbugs:findbugs'
      }
    }
    stage('Security Scan') {
            steps {
                echo 'Running security scan using OWASP ZAP'
            }
            post {
                success {
                    script {
                        def emailBody = "The security scan has passed.Build Status: ${currentBuild.currentResult}"
                        emailext body: emailBody, subject: "Security Scan Passed", to: "wobushidahundan@gmail.com"
                        emailext attachLog: true, subject: "Security Scan passed Log", to: "wobushidahundan@gmail.com"
                    }
                }
                failure {
                    script {
                        def emailBody = "The security scan has failed. Please check the logs for more details.Build Status: ${currentBuild.currentResult}"
                        emailext body: emailBody, subject: "Security Scan Failed", to: "wobushidahundan@gmail.com"
                        emailext attachLog: true, subject: "Security Scan Failed Log", to: "wobushidahundan@gmail.com"
                    }
                }
            }
        }
    stage('Deploy to Staging') {
      steps {
        echo 'Deploying to staging environment on AWS EC2 instance'
      }
    }
    stage('Integration Tests on Staging') {
      steps {
        sh 'mvn -f ../firstapp/pom.xml integration-test'
      }
    }
    stage('Deploy to Production') {
      steps {
        echo 'Deploying to production environment on AWS EC2 instance'
      }
    }
  }
}
