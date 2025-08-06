🚀 Build & Push Docker Image to Docker Hub using Jenkins Pipeline

This project demonstrates how to automate building a Docker image and pushing it to Docker Hub using a Jenkins CI/CD Pipeline.


---

🎯 Project Overview

In this project, we:

1. Build a Docker image for an application using Jenkins Pipeline.


2. Push the Docker image to Docker Hub automatically after a successful build.


3. Integrate Jenkins with Docker and Docker Hub for a fully automated CI/CD workflow.




---

🛠️ Tools & Technologies

Jenkins – CI/CD tool for automation

Docker – Containerization platform

Docker Hub – Container registry for storing images

GitHub – Source code repository for the project

Linux (Ubuntu/CentOS) – Jenkins server environment



---

📂 Project Structure

docker-jenkins-pipeline/
│
├── Jenkinsfile           # Pipeline script
├── Dockerfile            # Defines image build steps
├── app/                  # Application source code
└── README.md             # Project documentation


---

⚙️ Setup Instructions

1️⃣ Prerequisites

Install Jenkins on your server

Install Docker and give Jenkins user Docker access:

sudo usermod -aG docker jenkins

Install Docker Pipeline Plugin in Jenkins

Create a Docker Hub account and generate credentials in Jenkins



---

2️⃣ Configure Jenkins Credentials

1. Go to Jenkins Dashboard → Manage Jenkins → Credentials


2. Add Docker Hub username & password

ID: dockerhub-credentials

Username: <your-dockerhub-username>

Password: <your-dockerhub-password>





---

3️⃣ Jenkinsfile (Pipeline Script)

Here’s a sample Jenkinsfile for building and pushing the Docker image:

pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE = 'your-dockerhub-username/your-app'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/yourusername/docker-jenkins-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$BUILD_NUMBER .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                    docker push $DOCKER_IMAGE:$BUILD_NUMBER
                    docker tag $DOCKER_IMAGE:$BUILD_NUMBER $DOCKER_IMAGE:latest
                    docker push $DOCKER_IMAGE:latest
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Docker image pushed successfully to Docker Hub!"
        }
        failure {
            echo "❌ Build or Push failed!"
        }
    }
}


---

4️⃣ Trigger the Pipeline

1. Create a Pipeline Job in Jenkins.


2. Configure the Pipeline Script from SCM (GitHub).


3. Run the pipeline – it will:

Checkout code

Build Docker image

Push image to Docker Hub

🎯 Output

Docker image automatically built and pushed to Docker Hub.

Example image name:

your-dockerhub-username/your-app:1
your-dockerhub-username/your-app:latest



---

✅ Conclusion

This project demonstrates a simple yet effective CI/CD pipeline for building and pushing Docker images using Jenkins. It can be extended to deploy containers on servers or Kubernetes clusters.
