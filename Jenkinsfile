        pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'docker_username/image_name:tag'
        INVENTORY_FILE = 'ansible/inventory'
        PLAYBOOK_FILE = 'ansible/deploy_docker.yml'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials', 
                    url: 'https://github.com/your-username/repo-name.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-credentials', variable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u basilmohamed --password-stdin
                        docker push ${DOCKER_IMAGE}
                    '''
                }
            }
        }

       //optional Stage
stage('Debug Environment') {
    steps {
        sh 'env'
        sh 'cat ~/.ssh/known_hosts'
        sh 'ls -l ~/.ssh'
    }
}
       //optional Stage
stage('Verify Ansible Files') {
    steps {
        sh 'ls -l ansible'
        sh 'cat ansible/ansible.cfg'
    }
}


        stage('Deploy to VMs with Ansible') {
            steps {
                sh 'ansible-playbook -i ${INVENTORY_FILE} ${PLAYBOOK_FILE}'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
        }
