pipeline {
    agent {
        node {
            label 'slave-node'
        }
    }

    stages {
        stage('git-clone') {
            steps {
                git branch: 'main', url: 'https://github.com/DivineTaminang/tweet-trend-new.git'
            }            
        }
    }
}
