pipeline {
    agent {
        node {
            label 'slave-node'
        }
    }

    stages {
        stage('maven-build') {
            steps {
               sh 'mvn clean deploy'
            }            
        }
    }
}
