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

## 5. Make sure that your application runs smoothly

### 1. Install pm2

PM2 is a production process manager for Node.js applications with a built-in load balancer. It allows you to keep applications alive forever, to reload them without downtime and to facilitate common system admin tasks.

```bash
npm i pm2 -g
```

### 2. Starting your application

```bash
pm2 start app.js
```

Here app.js may change to your file name.

### Other pm2 commands

#### • To check the version of pm2

```bash
pm2 -v
```

#### • To stop pm2

```bash
pm2 stop app
```

**or this to stop all process**

```bash
pm2 stop all
```

#### • To restart pm2

```bash
pm2 restart app
```

**or this to restart all process**

```bash
pm2 restart all
```

#### • To see log

```bash
pm2 log
```

#### • To clear all process log

```bash
pm2 flush
```

**To clear certain process you can you its id replace `<id>` with the id of your process to clear it's log**

```bash
pm2 flush <id>
```

# Step 5: Install NGINX

## 1. Install NGINX

```bash
sudo apt install nginx
```

## 2. Configure NGINX to Proxy Pass to your Node.js Application

```bash
nano /etc/nginx/sites-available/default
```

**Press and hold ctrl + k to clear the Default file and add the following***

```bash
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
            proxy_pass http://localhost:8080;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
```

**Make sure to change port number for me I use localhost 8080 change the port to your port number**

## 3. To check NGINX config

```bash
sudo nginx -t
```

## 4. To restart the Nginx

```bash
sudo systemctl restart nginx
```

## 5. To check the Nginx status

```bash
sudo systemctl restart nginx
```

**or**

```bash
sudo nginx -s reload
```

# Step 5: Set up DNS with Route 53 🌐 (Before you begin, purchase a domain from Hostinger or GoDaddy)

## 1. Access Amazon Route 53

Open the Amazon Route 53 Console.

## 2. Create a Hosted Zone

- In the Route 53 dashboard, click on "Create Hosted Zone".
- Enter your domain name (e.g., yourdomain.com) and choose a region.
- Click on "Create."

## 3. Obtain Name Servers

After creating the hosted zone, note down the name server (NS) records provided by Route 53 (There will be 4 nameservers. Discard the dot at the end). These are the authoritative name servers for your domain.

## 4. Update Domain Registrar's Name Servers

- Log in to your domain registrar's website (the service where you purchased your domain, e.g., GoDaddy, Hostinger).
- Navigate to the DNS management or name server settings.
- Replace the existing name servers with the ones provided by Route 53.

## 5. Create Record Sets

- In the Route 53 dashboard, select the hosted zone you created.
- Click on "Create Record".
- For an A record (IPv4), set the following:  
          &nbsp;1. Name: (Leave it empty for the root domain, or enter a subdomain).  
          &nbsp;2. Type: A - IPv4 address  
          &nbsp;3. TTL: Choose an appropriate value (or leave the default) (By default it 300 hrs free a month).  
          &nbsp;4. Value: Enter the IPv4 address of your EC2 instance or other resource.  
- For a CNAME record, set the following:  
        &nbsp;1. Name: (Leave it empty for the root domain, or enter a subdomain).  
        &nbsp;2. Type: CNAME - Canonical name.  
        &nbsp;3. TTL: Choose an appropriate value (or leave the default) (By default it 300 hrs free a month).  
        &nbsp;4. Value: Enter the canonical name of your resource (e.g., your-load-balancer-url.elb.amazonaws.com).  
- Click on "Create" to save the record set.

## 6. Your DNS is configured , It might take sometime to update the nameserver

## 7. Test Your Domain

Open a web browser and navigate to your domain (e.g., http://example.com). Ensure that it resolves to the correct AWS resource.

# Step 6: Set Up SSL Certificate (http to https)

## 1. Install Certbot

```bash
sudo add-apt-repository ppa:certbot/certbot
```

```bash
sudo apt-get update
```

```bash
sudo apt-get install python3-certbot-nginx
```

## 2. Obtain SSL Certificate

Replace `example.com` and `www.example.com` with your actual domain names. Follow the prompts to complete the certificate issuance process. Certbot will automatically update the Nginx configuration to use the obtained SSL certificate (if you have only one url set up give only one).

```bash
sudo certbot --nginx -d example.com -d www.example.com
```

## 3.  Automatic Renewal

Certbot certificates only valid for 90 days, test the renewal process with, Certbot certificates need to be renewed periodically.

```bash
sudo certbot renew --dry-run
```
