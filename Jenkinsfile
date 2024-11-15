def frontendImage="bardys/frontend"
def backendImage="bardys/backend"

parameters {
    string(name: 'backendDockerTag', defaultValue: 'latest', description: 'Backend docker image tag')
    string(name: 'frontendDockerTag', defaultValue: 'latest', description: 'Frontend docker image tag')
}


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
        stage('Adjust version') {
            steps {
                script {
                    currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
                }
            }
        }
        stage ('remove') {
            steps {
                sh "docker rm -f frontend backend"
            }
        }


        stage('Deploy application') {
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                            sh "docker-compose up -d"
                    }
                }
            }}
        stage('tests') {
            steps {
                sh "pip3 install -r test/selenium/requirements.txt"
                sh "python3 -m pytest test/selenium/frontendTest.py"
            }
        }




}

post {
  always {
    sh "docker-compose down"
    cleanWs()
  }
}

}