def frontendImage="pandaacademy/frontend"
def backendImage="pandaacademy/backend"

pipeline {
    agent {
        label 'agent'
    }

    environment {
        PIP_BREAK_SYSTEM_PACKAGES = 1
    }
    
    parameters {
        string(name: 'backendDockerTag', defaultValue: 'latest', description: 'Backend docker image tag')
        string(name: 'frontendDockerTag', defaultValue: 'latest', description: 'Frontend docker image tag')
    }

    stages {
        stage('Get Code') {
            steps {
                checkout scm // Get some code from a GitHub repository
            }
        }
}}