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
    }
}
