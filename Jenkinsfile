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
        stage('Setup Test Environment') {
            steps {
                sh '''
                    python3 -m venv .venv
                    .venv/bin/python -m pip install pytest
                '''
            }
        }

        stage('Unit Testing') {
            steps {
                sh '''
                    mkdir -p test-results
                    .venv/bin/python -m pytest -v test_script2.py \
                        --junitxml=test-results/results.xml
                '''
            }
            post {
                always {
                    junit testResults: 'test-results/results.xml',
                        skipPublishingChecks: true
                    archiveArtifacts artifacts: 'test-results/results.xml',
                                    allowEmptyArchive: true
                }
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
