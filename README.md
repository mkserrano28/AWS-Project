# Continuous Integration and Continuous Delivery/Continuous deployment for React (Vite + Tailwind) 

Continuous Integration and Continuous Deployment (CI/CD) is essential for automating the deployment process of web applications. In this blog, we'll walk through setting up a CI/CD pipeline using AWS services to deploy a React (Vite + Tailwind CSS) application.

# Prerequisites

Before starting, ensure you have:
An AWS account
A GitHub repository with your React (Vite + Tailwind) project
AWS CLI configured on your local machine
Basic knowledge of AWS services

# Follow the steps to create CI/CD pipeline in this Folder 
# Also i will post here the step by step to create CI/CD pipeline

# Step 1: Set Up a GitHub Repository

1.Create a new repository on GitHub 
2. Push your React (Vite + Tailwind) project to your repository.

# Step 2: Create a S3 Bucket

1. Go to AWS Management Console.
2. Search for S3 in the AWS search bar and click S3 to open the S3 service.
3. Click on the Create bucket button.
4 .Enter a bucket name (e.g., my-static-site-bucket).
  The name must be unique across AWS.
  Use only lowercase letters, numbers, hyphens, and dots.
5. Choose a region close to your country
6. Scroll down to the Block Public Access settings for this bucket.
7. Ensure all  options are checked (default setting)
   uncheck public access
8. Click Create bucket.

# Step 3: Create a Code Build

1. Go to AWS Management Console.
2. Create Project
3. Create your project name
4. Source provider GitHub
5. Operating System
   Ubuntu
6. BuildSpec - Choose insert build commands

Here's the YAML File

  version: 0.2

phases:
  install:
    runtime-versions:
      nodejs: 22.x  # Use Node.js v22 (Ensure CodeBuild supports this version)
    commands:
      - echo "Installing dependencies..."
      - npm install

  build:
    commands:
      - echo "Building the React app..."
      - npm run build

artifacts:
  files:
    - '**/*'
  base-directory: dist  # Change to dist if using Vite
  
  cache:
    paths:
      - 'node-modules/**/*'


7. Atifact--- follow these steps
   1.Amazon S3
   2.your-bucket-name
   3.use your artifact you create in your s3
8. Namespace type - optional
   Choose Build ID
9. Create Project

# Step 4: Create IAM-Roles

1.Go to IAM Console: Open the AWS IAM Console.
2.Click "Roles" in the left sidebar.
3.Click "Create role".
4.Choose Trusted Entity:
  Select Custom trust policy (instead of AWS service).

5.Add the Trust Policy:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "codepipeline.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}




6. Click "Next" to Attach Policies:
  Attach the following AWS-managed policies:
  AWSCodePipelineFullAccess
  AWSCodeBuildAdminAccess (if using CodeBuild)
  AmazonS3FullAccess (if using S3)
  AWSCodeDeployFullAccess (if using CodeDeploy)
  
7.Click "Next" and enter a Role Name, e.g., CodePipelineServiceRole.
8.Click "Create role".


# Step 5: Create Code Pipeline

1. Go to IAM Console: Search Code Pipeline
2. Create Option -- Build Custom Pipeline - next
3. Pipeline name -- name of your pipeline project
4. Service role -- Existing role- choose your IAM- role your create in the step 4
   enter the Role in Role ARN
5.Source -- Amazon S3 follow this steps:
   Github(via Github App)
   Connect your github
   choose your repository
   branch - your branch name in your repository
6. Build Option
   Choose -- other build providers
   AWS CodeBuild
   choose your code build project
7. Deployment Option
    I choose S3
8. Review your pipeline
9. Create pipeline


   
   







