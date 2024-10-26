pipeline {
    agent {
        node {
            label 'maven'
    
        }
    } 
environment {
    PATH= "/opt/apache-maven-3.9.9/bin:$PATH"
}

    stages {
        stage("mavenDEV-builddev") {
            steps {
               sh 'mvn clean deploy'
            }            
        }
    }
}
