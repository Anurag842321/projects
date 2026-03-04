pipeline {
    agent {
      label 'master.tg.com'
    }
    environment{
        IMAGE_NAME = "933538/via-jenkins"
        TAG = "${BUILD_NUMBER}"
    } 
    stages {
        stage('Chechout From GitHub') {
            steps {
                git credentialsId: 'd7600991-23ca-479e-a2a2-6541db1321ef',
                    url: 'https://github.com/Anurag842321/projects.git',
                    branch: 'main'
            }
        }
        stage('Find Dockerfile') {
            steps {
            sh """
             echo "Seraching Dockerfile"
             find . -name Dockerfile
            """
            }
        }
        stage('Build Docker-Image') {
            steps {
            sh """
            docker build -t $IMAGE_NAME:$TAG -f tomcat-server-with-nginx-proxy/Dockerfile tomcat-server-with-nginx-proxy 
            """
            }
        }
        stage('DockerHub Login') {
            steps {
               withCredentials([usernamePassword(
                   credentialsId: 'docker-hub',
                   usernameVariable: 'DOCKER_USER',
                   passwordVariable: 'DOCKER_PASS'
            )]) {
                sh"""
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                """
               }
            }
        }
    stage('Push Image to DockerHub') {
            steps {
            sh"""
            docker image push $IMAGE_NAME:$TAG 
            """
            }
        }
    stage('This Stage is For K8s Steps') {
            steps {
            sh"""
            echo "updating Image Placeholder in YAML..."
            sed -i "s/BUILD_NUMBER/${TAG}/g" tomcat-deploy.yml

            echo "Deploying YAML Files"
            kubectl apply -f tomcat-server-with-nginx-proxy/

            echo "Deployment Status"
            kubectl get all -n mytg
            kubectl get svc -n mytg
            """
            }
        }
    }
}

