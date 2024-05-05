pipeline {
    agent any

    stages {
        //stage('Checkout code') {
            //steps {
                //checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Rahul-training/Devops.git']])
           // }
       // }
     stage('Install Maven Build Tool') {
            steps { 
                sh 'wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz'
                sh 'tar -xzvf /var/lib/jenkins/workspace/test/apache-maven-3.9.4-bin.tar.gz'
             } 
           }
     stage('Compile Sample Application') {
            steps {
                dir('/var/lib/jenkins/workspace/test/addressbook/addressbook_main'){
                sh '/var/lib/jenkins/workspace/test/apache-maven-3.9.4/bin/mvn compile'
            }
           }
        }
    stage('Test Sample Application') {
            steps {
                dir('/var/lib/jenkins/workspace/test/addressbook/addressbook_main'){
                sh '/var/lib/jenkins/workspace/test/apache-maven-3.9.4/bin/mvn test'
            }
           }
        }
     stage('Package Sample Application') {
            steps {
                dir('/var/lib/jenkins/workspace/test/addressbook/addressbook_main'){
                sh '/var/lib/jenkins/workspace/test/apache-maven-3.9.4/bin/mvn package'
            }
           }
        }
     stage('Tomcat Prerequisites Installation') {
            steps {
                sh 'sudo apt update'
                sh 'sudo apt install openjdk-17-jre -y'
           }
        }

     stage('Install Apache Tomcat Server') {
            steps {
                sh 'wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz'
                sh 'tar -xzvf apache-tomcat-8.5.24.tar.gz'
                sh 'sudo chown -R cloudadmin:cloudadmin apache-tomcat-8.5.24'
                sh 'sudo cp tomcat-users.xml apache-tomcat-8.5.24/conf/tomcat-users.xml'
                sh 'sudo cp context.xml apache-tomcat-8.5.24/webapps/manager/META-INF/context.xml'
                sh 'sudo sed -i "s/8080/8081/g" apache-tomcat-8.5.24/conf/server.xml'
        }
}        
     stage('Deploy Sample Application To Tomcat Server') {
            steps {
                sh 'sudo cp addressbook/addressbook_main/target/addressbook.war apache-tomcat-8.5.24/webapps/'
                sh 'sudo runuser -l cloudadmin -c "/var/lib/jenkins/workspace/test/apache-tomcat-8.5.24/bin/startup.sh"'
            }
        }
    }
    }    
