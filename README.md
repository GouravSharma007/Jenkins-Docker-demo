# 🐳 Jenkins Docker CI/CD Pipeline - WebApp Demo

This project demonstrates a **complete DevOps CI/CD pipeline** using:

- 🧑‍💻 GitHub (for source code)
- 🐳 Docker (for containerizing the app)
- 🛠️ Jenkins (for automation)
- ☁️ Docker Hub (for image registry)
  
-----------------------------------------------------------------------------------------------

## 📁 Project Structure

Jenkins-Docker-demo/

├── Dockerfile

├── index.html

├── README.md

├── style.css

└── script.js

---

## ⚙️ Tech Stack

- ✅ HTML/CSS/JS
- ✅ Docker
- ✅ Jenkins
- ✅ Git & GitHub
- ✅ Docker Hub

---

## 🚀 CI/CD Pipeline Workflow

1. **Code Push**: Code is pushed to GitHub.
2. **Jenkins Job**: Jenkins pulls the code.
3. **Build Image**: Docker builds an image from the repo.
4. **Push Image**: Jenkins pushes image to Docker Hub.
5. **Deploy**: Jenkins runs a new container from that image.

---

## 📦 Docker Image

To pull and run this image manually:

docker pull gourav0/mywebapp
docker run -d -p 8080:80 gourav0/mywebapp
Then open: http://localhost:8080 in your browser.

🧰 How to Set Up Jenkins Pipeline
Open Jenkins on your EC2 instance:
http://<your-ec2-public-ip>:8080

Create a new pipeline job:
Name: anything you want
Type: Pipeline
Paste the code from Below and do some changes like add credention

Add DockerHub credentials in Jenkins:
Go to: Manage Jenkins > Credentials > Global > Add Credentials
Type: Secret Text
ID: docker-hub-pass
Secret: your DockerHub password or access token

Run the job:
Jenkins will pull the repo, build and push Docker image, and run it on EC2.

🔍 Jenkinsfile Breakdown
```bash
pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'gourav0'
        IMAGE_NAME = 'mywebapp'
    }

    stages {
        stage('Clone GitHub Repository') {
            steps {
                git 'https://github.com/GouravSharma007/Jenkins-Docker-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $YOUR_DOCKER_HUB_NAME/$IMAGE_NAME .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([string(credentialsId: 'docker-hub-pass', variable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $YOUR_DOCKER_HUB_NAME --password-stdin'
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                sh 'docker push $YOUR_DOCKER_HUB_NAME/$IMAGE_NAME'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker rm -f mywebapp-container || true
                    docker run -d -p 8080:80 --name mywebapp-container $YOUR_DOCKER_HUB_NAME/$IMAGE_NAME
                '''
            }
        }
    }
}

```

✅ To-Do After Setup
Task	Status
GitHub Repo Created	✅

Docker Image Built & Pushed	✅

Jenkins Installed	✅

Jenkinsfile Added	✅

Jenkins Job Created	✅

Pipeline Run & Verified	✅

👀 Future Enhancements
* Add GitHub Webhook for auto-triggering builds
* Integrate Slack notifications for pipeline status
* Use GitHub Actions or GitLab CI as alternative CI tools
* Deploy using Nginx reverse proxy for multiple apps

Author: Gourav Sharma📷 Output Preview
Once deployed, visit:

http://<your-ec2-ip>:8080

I have added some images in the repo, please check the change.
