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
         stage ('Deploy Image') {
            steps {
                    sh 'docker create container --publish 8080:8080 --name devopsdemo testdeploy' 
            }
        }
    }
}
