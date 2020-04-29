pipeline {
    agent {label 'aws_slave'}
    tools {
        maven 'maven'
        jdk 'java'
    }
    options {
    skipDefaultCheckout(true)
}
    stages {
        stage ('Checkout') {
            steps {
                
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
        }
    }
}
