pipeline {
    agent any

    stages {
        stage('Build Backend Image') {
            steps {
                // Building the backend image using the local 'backend' folder
                sh 'docker build -t backend-app backend'
            }
        }

        stage('Deploy Backends') {
            steps {
                // Removing old containers if they exist to avoid name conflicts
                sh 'docker rm -f backend1 backend2 || true'
                // Running two instances of the backend service
                sh 'docker run -d --name backend1 backend-app'
                sh 'docker run -d --name backend2 backend-app'
                // Adding a 3-second delay to let containers start
                sh 'sleep 3'
            }
        }

        stage('Deploy NGINX') {
            steps {
                // Removing old NGINX container
                sh 'docker rm -f nginx-lb || true'
                // Mounting nginx.conf from your repo to the container's default config path
                sh 'docker run -d --name nginx-lb -p 80:80 -v $(pwd)/nginx/nginx.conf:/etc/nginx/conf.d/default.conf nginx'
                // Adding a 2-second delay to ensure NGINX is ready
                sh 'sleep 2'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully, NGINX load balancer is running.'
        }
        failure {
            echo 'Pipeline failed. Check console logs for errors.'
        }
    }
}
