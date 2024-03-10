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
- **Configuring Network Settings and Permissions :** Now, let’s ensure your EC2 instance has the necessary network settings and permissions to communicate with the outside world. Check Allow SSH traffic from, Allow HTTPS traffic from the internet and Allow HTTP traffic from the internet are enabled or selected.
- Finally, click "Launch".

# Step 2: Connect to Your EC2 Instance

## 1. Navigate to EC2 Dashboard

Go to the AWS Management Console.

## 2. Select Your EC2 Instance

- Click on "Instances" in the EC2 Dashboard.
- Choose the instance you want to connect to.

## 3. Access the Connect Tab (EC2 Instance Connect)

In the lower panel, click on the "Connect" tab.

# Step 3: Set Up Node.js on EC2 Instance

## 1. Update the Package List

**Update package lists:** It’s always a good practice to update the package lists to ensure you’re installing the latest versions of software.

```bash
sudo apt update
```

## 2. Upgrade Existing Packages

```bash
sudo apt upgrade -y
```

## 3. Install NVM

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

## 4. Source NVM script

```bash
source ~/.bashrc
```

To see installed version of NVM type

```bash
nvm -v
```

## 5. Install Node.js using NVM

Once NVM is installed, you can install Node.js using it. First, check the available Node.js versions.

```bash
nvm ls-remote
```

**Choose a version to install:** Select the Node.js version you want to install. For example, to install Node.js version 20.11.1 which is the current LTS vertion of Node.js

```bash
nvm install 20.11.1
```

**Verify installation:** After installation, you can verify that Node.js and NPM (Node Package Manager) are installed correctly by checking their versions.

```bash
node -v
```

```bash
npm -v
```

# Step 4: Clone your Node.js Project to EC2

## 1. Clone your application from GitHub

Replace `<your-git-repository-url>` with the URL of your Git repository.

```bash
git clone <your-git-repository-url>
```

## 2. Navigate into the cloned directory

```bash
cd your-project-directory
```

## 3. Install Dependencies

```bash
npm install
```

**or**

```bash
npm i
```

## 4. Setup your .env file

Use nano command to create or directly start typing to modify your file.

```bash
nano .env
```

## 5. : Make sure that your application runs smoothly

## 1. Install pm2

PM2 is a production process manager for Node.js applications with a built-in load balancer. It allows you to keep applications alive forever, to reload them without downtime and to facilitate common system admin tasks.

```bash
npm i pm2 -g
```

## 2. Starting your application

```bash
pm2 start app.js
```

Here app.js may change to your file name.
