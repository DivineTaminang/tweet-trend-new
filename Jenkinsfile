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
               echo "-------maven build started------"
               sh 'mvn clean deploy'
                echo "-------maven build ended------"
            }            
        }

        stage('unit test') {
            steps {
            echo "-------unit test started------"
            sh 'mvn surefire-report:report'
             echo "-------unit test ended------"
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
}

