pipeline {
    agent any
    
    parameters {
        string(name: 'TARGET_DATE', defaultValue: '', description: 'Target date for finding commits')
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                checkout scm
            }
        }
        
        stage('Find Commits') {
            steps {
                script {
                    def currentDate = params.TARGET_DATE
                    def previousCommit = sh(script: "git rev-list -n 1 --before='${currentDate} 00:00' HEAD", returnStdout: true).trim()
                    def currentCommit = sh(script: "git rev-list -n 1 --before='${currentDate} 23:59' HEAD", returnStdout: true).trim()
                    
                    echo "Previous commit: ${previousCommit}"
                    echo "Current commit: ${currentCommit}"
                }
            }
        }
        
        stage('Diff Commits') {
            steps {
                script {
                    def diffOutput = sh(script: "git diff ${previousCommit}..${currentCommit}", returnStdout: true).trim()
                    
                    echo "Difference between commits:"
                    echo "${diffOutput}"
                }
            }
        }
    }
}

