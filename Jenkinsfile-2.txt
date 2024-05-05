pipeline {
    agent {
        label 'testslave'
    }

    stages {
        stage('Checkout code') {
            steps {
                checkout scmGit(branches: [[name: '*/v24']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vaadin/bookstore-example.git']])
            }
        }
     stage('Install Maven Build Tool') {
            steps { 
                sh 'wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz'
                sh 'tar -xzvf /tmp/workspace/test/apache-maven-3.9.4-bin.tar.gz'
             } 
           }
           stage('Compile Sample Application') {
            steps {
                dir('/tmp/workspace/test'){
                sh '/tmp/workspace/test/apache-maven-3.9.4/bin/mvn compile'
            }
           }
        }
    stage('Test Sample Application') {
            steps {
                dir('/tmp/workspace/test'){
                sh '/tmp/workspace/test/apache-maven-3.9.4/bin/mvn test'
            }
           }
        }
     stage('Package Sample Application') {
            steps {
                dir('/tmp/workspace/test'){
                sh '/tmp/workspace/test/apache-maven-3.9.4/bin/mvn package'
            }
           }
        }
        stage('Deploy Sample Application') {
            steps {
                dir('/tmp/workspace/test'){
                sh 'sudo runuser -u cloudadmin -- nohup /tmp/workspace/test/apache-maven-3.9.4/bin/mvn jetty:run > output.log 2>&1 &'
                sh 'sleep 120'
            }
           }
        }
    }
}