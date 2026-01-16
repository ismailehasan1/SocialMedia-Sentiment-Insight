
# 🎬 SocialMedia-Sentiment-Insight

Analyze YouTube comments and predict their sentiment using Python, DVC, and AWS. Fully deployable with Docker and CI/CD pipelines.

# 🌟 Features
✅ Sentiment analysis on YouTube comments
✅ Local API demo with JSON requests
✅ Version-controlled data pipelines using DVC
✅ Dockerized deployment to AWS EC2
✅ CI/CD with GitHub Actions


# 🛠 Environment Setup
## Create and activate Conda environment
conda create -n youtube python=3.11 -y
conda activate youtube

## Install dependencies
pip install -r requirements.txt


# 📊 DVC Workflow
## Initialize DVC
dvc init
## Reproduce pipeline
dvc repro
## Visualize DAG
dvc dag

# 🎥 YouTube API

Get your YouTube API key to fetch comments:
YouTube API Key Setup

# 🚀 Demo

Run the API locally:

http://localhost:5000/predict


Example JSON request in Postman:

{
  "comments": [
    "This video is awesome! I loved it a lot",
    "Very bad explanation. Poor video"
  ]
}

# ☁️ AWS Deployment
## 1️⃣ Setup IAM User

Create IAM user with deployment access

Attach policies:

AmazonEC2ContainerRegistryFullAccess

AmazonEC2FullAccess

## 2️⃣ Elastic Container Registry (ECR)

Create ECR repository to store Docker images

Example URI:

315865595366.dkr.ecr.us-east-1.amazonaws.com/youtube

## 3️⃣ EC2 Deployment

Launch an Ubuntu EC2 instance

Install Docker:

sudo apt-get update -y
sudo apt-get upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker

# 🐳 Docker Deployment
## Build Docker image
docker build -t youtube-sentiment .

## Authenticate and push to ECR
aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <ECR_URI>
docker tag youtube-sentiment <ECR_URI>:latest
docker push <ECR_URI>:latest

## Pull and run on EC2
docker pull <ECR_URI>:latest
docker run -p 5000:5000 <ECR_URI>:latest

# ⚡ GitHub Actions CI/CD

Set up self-hosted runner on EC2:

Settings > Actions > Runners > New self-hosted runner

Follow setup commands

Add GitHub Secrets:

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=us-east-1
AWS_ECR_LOGIN_URI=<ECR_URI>
ECR_REPOSITORY_NAME=<ECR_REPO_NAME>


Workflow automatically:

Builds Docker image

Pushes to ECR

Deploys on EC2

# 📈 Project Workflow Diagram
flowchart LR
    A[Fetch YouTube Comments] --> B[Preprocess Data]
    B --> C[Sentiment Model Prediction]
    C --> D[Store Results]
    D --> E[Deploy API via Docker]
    E --> F[AWS EC2 Deployment]




