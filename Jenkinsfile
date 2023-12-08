git add .

    agent {
        node {
            label 'slave-node'
    
        }
    } 
environment {
    PATH= "/opt/apache-maven-3.9.6/bin:$PATH"
}

    stages {
        stage("maven-build") {
            steps {
               sh 'mvn clean deploy'
            }            
        }
    }
}