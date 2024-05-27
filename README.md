# Clone this project
```
git clone https://github.com/atulkamble/AWSWebsiteProject.git
cd AWSWebsiteProject
```
# AWSWebsiteProject
Hosting a Website in AWS EC2 Service
detailed project for setting up and hosting a website on AWS EC2, including code snippets for configuration and deployment.

---

### **Project: Hosting a Website on AWS EC2**

**Objective:**  
Set up and host a simple static website on an AWS EC2 instance.

---

### **Prerequisites**

1. **AWS Account:** Create an AWS account if you don’t have one.
2. **AWS CLI:** Install and configure the AWS Command Line Interface (CLI). [Download](https://aws.amazon.com/cli/)
3. **EC2 Key Pair:** Create an EC2 key pair to access your instance.
4. **Git**
   ```
   git config --global user.name "username"
   git config --global user.email "email@gmail.com"
   ```

---

### **Step 1: Launch an EC2 Instance**

1. **Log in to AWS Management Console** and navigate to the EC2 Dashboard.

2. **Launch Instance:**
   - Click on “Launch Instance”.
   - Choose an Amazon Machine Image (AMI): Select the “Amazon Linux 2 AMI”.
   - Choose an Instance Type: Select “t2.micro” (free tier eligible).
   - Configure Instance Details: Accept the default settings.
   - Add Storage: Accept the default settings.
   - Add Tags: (Optional) Add a tag like `Name: EC2WebServer`.
   - Configure Security Group: Create a new security group and add rules to allow HTTP (port 80) and SSH (port 22) traffic.
   - Review and Launch: Review your settings and click “Launch”. Select your key pair and launch the instance.

---

### **Step 2: Connect to Your EC2 Instance**

1. **Open your terminal** (Linux/Mac) or use PuTTY (Windows).

2. **Connect using SSH:**
   ```bash
   ssh -i /path/to/your-key-pair.pem ec2-user@your-ec2-public-dns
   ```

---

### **Step 3: Install a Web Server**

1. **Update the instance and install Apache:**
   ```bash
   sudo yum update -y
   sudo yum install -y httpd
   ```

2. **Start the Apache service and enable it to start on boot:**
   ```bash
   sudo systemctl start httpd
   sudo systemctl enable httpd
   sudo usermod -a -G apache ec2-user
   ```

---

### **Step 4: Deploy Your Website**

1. **Create a simple HTML file:**
   ```bash
   sudo echo "<html>
   <head>
       <title>My First AWS Website</title>
   </head>
   <body>
       <h1>Hello, World!</h1>
       <p>Welcome to my website hosted on AWS EC2!</p>
   </body>
   </html>" > /var/www/html/index.html
   ```

2. **Adjust file permissions (if needed):**
   ```bash
   sudo chmod 644 /var/www/html/index.html
   ```

3. **Open your browser and navigate to your EC2 instance’s public DNS:**
   ```
   http://your-ec2-public-dns
   ```

   You should see your “Hello, World!” webpage.

---

### **Step 5: (Optional) Automate Deployment with a Script**

1. **Create a deployment script (deploy_website.sh):**
   ```bash
   #!/bin/bash
   sudo yum update -y
   sudo yum install -y httpd
   sudo systemctl start httpd
   sudo systemctl enable httpd
   sudo usermod -a -G apache ec2-user

   echo "<html>
   <head>
       <title>My First AWS Website</title>
   </head>
   <body>
       <h1>Hello, World!</h1>
       <p>Welcome to my website hosted on AWS EC2!</p>
   </body>
   </html>" > /var/www/html/index.html

   sudo chmod 644 /var/www/html/index.html
   ```

2. **Upload and execute the script on your EC2 instance:**
   ```bash
   scp -i /path/to/your-key-pair.pem deploy_website.sh ec2-user@your-ec2-public-dns:/home/ec2-user/
   ssh -i /path/to/your-key-pair.pem ec2-user@your-ec2-public-dns 'bash /home/ec2-user/deploy_website.sh'
   ```

---

### **Conclusion**

You have successfully set up and hosted a static website on an AWS EC2 instance. This project covers the basics of launching an EC2 instance, installing a web server, and deploying a simple HTML page. For more complex websites, you can expand this setup by integrating other services like databases, load balancers, or using infrastructure as code tools like Terraform or Ansible.
