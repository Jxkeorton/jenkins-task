pipeline {
    agent { label 'python-agent-template' }

    stages {
        stage('Checkout') {
            steps {
                echo 'Downloading project from GitHub'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'python3 -m py_compile script1.py script2.py'
                sh 'tar -czf python-app.tar.gz script1.py script2.py'
            }
        }
        stage('Run') {
            steps {
                sh 'python3 script1.py'
            }
        }
    }
    post {
        success { archiveArtifacts artifacts: 'python-app.tar.gz' }
    }
}
