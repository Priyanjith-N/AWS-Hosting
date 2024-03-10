# Project Deployment on AWS EC2

This guide, easy to follow for anyone, shows how to put your Node.js app on Amazon's cloud (AWS) using an EC2 machine. It'll help you set things up and get your app running!

# Step 1: Set Up an AWS EC2 Instance

## 1. Sign in to AWS Console

Log in to your AWS account at https://aws.amazon.com/.

## 2. Navigate to EC2 Dashboard

Go to the EC2 Dashboard

## 3. Launch an Instance

- Click on the "Launch Instance" button.
- Choose an Amazon Machine Image (AMI), such as Amazon Linux or Ubuntu.
- Select an instance type based on your project requirements.
- Here we are setting up EC2 instance with the Ubuntu 64-bit operating system and the t2.micro instance type, which is eligible for the AWS Free Tier.
- **Creating and Downloading Your PEM File :** To securely connect to your EC2 instance, you’ll need a PEM (Privacy Enhanced Mail) file containing your private key.
- **Configuring Network Settings and Permissions :** Now, let’s ensure your EC2 instance has the necessary network settings and permissions to communicate with the outside world. Check Allow SSH traffic from, Allow HTTPS traffic from the internet and Allow HTTP traffic from the internet are enabled.
- Finally, click "Launch".

# Step 2: Connect to Your EC2 Instance
