SocialMedia-Sentiment-Insight

A sentiment analysis project for YouTube comments, leveraging Python, DVC, and AWS for deployment. The system predicts sentiment for comments and can be deployed using Docker on AWS EC2 with CI/CD via GitHub Actions.

Table of Contents

Environment Setup

DVC Workflow

YouTube API

Demo

AWS Deployment

Docker Deployment

GitHub Actions CI/CD

Environment Setup

Create a Conda environment and install dependencies:

conda create -n youtube python=3.11 -y
conda activate youtube
pip install -r requirements.txt

DVC Workflow

Initialize DVC:

dvc init


Reproduce pipeline stages:

dvc repro


Visualize pipeline:

dvc dag

YouTube API

To fetch comments for sentiment analysis, you need a YouTube API key.

Follow this guide: Get YouTube API Key from GCP

Demo

Run the API locally:

http://localhost:5000/predict


Example JSON request in Postman:

{
  "comments": [
    "This video is awesome! I loved it a lot",
    "Very bad explanation. Poor video"
  ]
}

AWS Deployment
Prerequisites

Login to AWS Console

Create an IAM user with deployment access

Policies required:

AmazonEC2ContainerRegistryFullAccess

AmazonEC2FullAccess

Steps

ECR (Elastic Container Registry)

Create a repository to store Docker images

Save the URI, e.g.:

315865595366.dkr.ecr.us-east-1.amazonaws.com/youtube


EC2 (Ubuntu Machine)

Launch an EC2 instance

Install Docker:

sudo apt-get update -y
sudo apt-get upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker

Docker Deployment

Build Docker image:

docker build -t youtube-sentiment .


Push image to ECR:

aws ecr get-login-password --region <AWS_REGION> | docker login --username AWS --password-stdin <ECR_URI>
docker tag youtube-sentiment <ECR_URI>:latest
docker push <ECR_URI>:latest


Pull and run image on EC2:

docker pull <ECR_URI>:latest
docker run -p 5000:5000 <ECR_URI>:latest

GitHub Actions CI/CD

Set up EC2 as a self-hosted runner:

Navigate to Settings > Actions > Runners > New self-hosted runner

Choose OS and follow setup commands

Add GitHub secrets:

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=us-east-1
AWS_ECR_LOGIN_URI=<ECR_URI>
ECR_REPOSITORY_NAME=<ECR_REPO_NAME>


Workflow automates:

Docker image build

Push to ECR

Deploy to EC2
