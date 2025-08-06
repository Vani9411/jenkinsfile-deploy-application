🚀 Build & Push Docker Image to Docker Hub using Jenkins Pipeline

In this project, I automated the process of building a Docker image and pushing it to Docker Hub using a Jenkins pipeline.
This setup helps to create a simple CI/CD flow for containerized applications.


---

✨ What This Project Does

Automatically builds a Docker image whenever the pipeline runs

Pushes the image to Docker Hub with version tags and latest tag

Makes it easy to integrate with future deployments like Kubernetes or cloud servers



---

🛠 Tools I Used

Jenkins – To create and manage the CI/CD pipeline

Docker – To containerize the application

Docker Hub – To store and manage Docker images

GitHub – To host the source code

Linux (Ubuntu) – For the Jenkins and Docker environment



---

📂 Project Structure

docker-jenkins-pipeline/
│
├── Jenkinsfile           # My Jenkins pipeline script
├── Dockerfile            # Instructions for building the Docker image
├── app/                  # Application source code
└── README.md             # Project documentation


---

⚙ Steps I Followed

1️⃣ Install Prerequisites

Installed Jenkins and Docker on my Linux server

Added Jenkins user to the Docker group:

sudo usermod -aG docker jenkins

Installed Docker Pipeline Plugin in Jenkins



---

2️⃣ Configure Docker Hub Credentials in Jenkins

1. Open Jenkins → Manage Jenkins → Credentials


2. Add new credentials with:

ID: dockerhub-credentials

Username: <my-dockerhub-username>

Password: <my-dockerhub-password>



---

3️⃣ My Jenkinsfile

This pipeline script builds and pushes the Docker image:

pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE = 'mydockerusername/my-webapp'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/myusername/docker-jenkins-pipeline.git'
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
            echo " Docker image successfully pushed to Docker Hub!"
        }
        failure {
            echo " Something went wrong. Check the logs."
        }
    }
}

---

🎯 Output

After the pipeline runs, the Docker image will be available on my Docker Hub:

mydockerusername/my-webapp:1
mydockerusername/my-webapp:latest


---

🏆 Conclusion

I built this project to practice DevOps CI/CD with Docker and Jenkins.
Now, whenever I push updates, I can quickly build and store versioned Docker images automatically.
