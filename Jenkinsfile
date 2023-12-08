pipeline {
    agent {
        node {
            label 'slave-node'
    
        }
    } 
environment {
    PATH= "/opt/apache-maven-3.9.6/bin:$PATH"
}

    stages {
        stage("mavenM-build") {
            steps {
               sh 'mvn clean deploy'
            }            
        }
    }
}
