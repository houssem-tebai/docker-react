def commit_id
pipeline {
    agent any
    stages {
        stage('preparation') {
            steps {
                checkout scm
                sh "git rev-parse --short HEAD > .git/commit-id"
                script {
                    commit_id = readFile('.git/commit-id').trim()
                }
            }
        }

        stage ("image build") {
            steps {
                echo 'building docker image'
                sh "docker build -t webapp:${commit_id} ."
                echo 'docker image built'
            }
        }
        stage('deploy') {
            steps {
                sh "docker run -d -p 80:80 webapp:${commit_id}"
            }
        }
    }
}
