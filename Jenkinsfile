pipeline {
    agent any

    parameters {
        string(name: 'IMAGE_TAG', defaultValue: 'latest', description: 'Docker image tag to deploy')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Files') {
            steps {
                sh 'ls -R'
            }
        }

        stage('Check Ansible') {
            steps {
                sh 'ansible --version || true'
            }
        }

        stage('Run Playbook') {
            steps {
                sh 'ansible-playbook ansible/playbook.yml -i inventory.ini -e image_tag=${IMAGE_TAG}'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                echo "Waiting for app to start..."
                for i in 1 2 3 4 5; do
                  sleep 5
                  if curl -f http://localhost/docs; then
                    echo "App is up"
                    exit 0
                  fi
                done
                echo "App did not become ready in time"
                exit 1
                '''
            }
        }

    }
}
