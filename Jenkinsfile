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
                checkout changelog: false, poll: false, 
                    scm: [$class: 'GitSCM', branches: [[name: '*/master']], 
                          doGenerateSubmoduleConfigurations: false, extensions: [], 
                          submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/Mahi51/testproject.git']]]
            }
        }

        stage ('Build Artifact') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install' 
            }
         }
        stage ('Build Image') {
            steps {
                    sh 'docker build -t testdeploy .' 
            }
        }
         stage ('Removing the container Image') {
            steps {
                    sh 'docker  stop devopsdemo' 
                    sh 'docker rm devopsdemo'
            }
        }
         stage ('Deploy war into container') {
            steps {
                    sh 'docker  container create --publish 8080:8080 --name devopsdemo testdeploy' 
                    sh 'docker start devopsdemo'
            }
        }
    }
}
