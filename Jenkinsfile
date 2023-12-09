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

          stage("SonarQube analysis") {
            environment { 
             scannerHome = tool 'mavine-sonar-scanner'
            }
            steps {  
              withSonarQubeEnv("mavine-sonarqube-server") { // If you have configured more than one global server connection, you can specify its name
              sh "${scannerHome}/bin/sonar-scanner"
            }
            }  
        }
}

