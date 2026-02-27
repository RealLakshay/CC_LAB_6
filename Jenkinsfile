pipeline {
    agent any

    stages {
        stage('Build Backend Image') {
            steps {
                // Building the backend image from the local backend folder
                sh 'docker build -t backend-app backend' [cite: 555]
            }
        }

        stage('Deploy Backends') {
            steps {
                // Removing old containers if they exist [cite: 568]
                sh 'docker rm -f backend1 backend2 || true' [cite: 568]
                // Running two instances of the backend [cite: 353, 354]
                sh 'docker run -d --name backend1 backend-app' [cite: 569]
                sh 'docker run -d --name backend2 backend-app' [cite: 354]
                // Delay to allow backends to start before NGINX reloads [cite: 757]
                sh 'sleep 3' [cite: 757]
            }
        }

        stage('Deploy NGINX') {
            steps {
                // Removing old NGINX container [cite: 574]
                sh 'docker rm -f nginx-lb || true' [cite: 574]
                // Running NGINX and mounting the local config file [cite: 490, 578]
                // Note: Path is updated to reflect your GitHub structure
                sh 'docker run -d --name nginx-lb -p 80:80 -v $(pwd)/nginx/default.conf:/etc/nginx/conf.d/default.conf nginx' [cite: 490, 578]
                // Short delay to ensure NGINX is ready [cite: 758]
                sh 'sleep 2' [cite: 758]
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully, NGINX load balancer is running.' [cite: 469]
        }
        failure {
            echo 'Pipeline failed. Check console logs for errors.' [cite: 746]
        }
    }
}
