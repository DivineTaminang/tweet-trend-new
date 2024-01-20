def registry = 'https://mavine01.jfrog.io'
def imageName = 'mavine-docker-local/ttrend'
def version   = '2.1.2'
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
            // No need to occupy a node
         stage("Quality Gate"){
            steps {
                script {
           timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
            def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
            if (qg.status != 'OK') {
            error "Pipeline aborted due to quality gate failure: ${qg.status}"
           }
          }
          }
       }
      }

     stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish is Started ---------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog_creds"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "mavine-libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended ----------------->'  
            
            }
        }   
    }

  
    stage(" Docker Build ") {
      steps {
        script {
           echo '<------------- Docker Build Started ------------>'
           app = docker.build(imageName+":"+version)
           echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }
            stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started ----------------->'  
                docker.withRegistry(registry, 'jfrog_creds'){
                    app.push()
                }    
               echo '<----------- Docker Publish Ended ---------------->'  
            }
        }
    }

stage  ("Deploy"){
steps {
  script { 
    echo '< ------------Helm deploy started ----------------->'
       sh 'helm install ttrend ttrend-0.1.0.tgz'
     echo '< -------------Helm deploy ended ----------------->'

  }
  

}
}
   

      }
    }   

