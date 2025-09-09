# Jenkins on AWS ec2

This guide provides step-by-step instructions for deploying Jenkins on an AWS EC2 instance. Jenkins is an open-source automation server used for CI/CD pipelines.

## 📝 Prerequisites

- 🔑 **AWS Account:** Sign in to your AWS account.
- 👤 **IAM Permissions:** Ensure you can create EC2 instances, security groups, and key pairs.
- 🗝️ **SSH Key Pair:** Prepare an SSH key pair for accessing your EC2 instance.

## 🛠️ Steps to Deploy Jenkins

### 1️⃣ Launch an EC2 Instance

1. 🖥️ Log in to the AWS Management Console.
2. 🚀 Go to **EC2** > **Instances** > **Launch Instance**.
3. 🏷️ Select an OS image:
    - **Amazon Linux 2** or **Ubuntu 22.04** (recommended)
4. ⚙️ Choose an instance type (e.g., `t2.micro` for testing).
5. ✏️ Configure details (default settings are fine for most cases).
6. 🗂️ Add storage if required (default is usually sufficient).
7. 🏷️ Add tags if you want to organize resources.
8. 🔒 Configure Security Group:
    - **Inbound Rules:**
        - SSH: TCP 22, Source: your IP
        - HTTP: TCP 8080, Source: 0.0.0.0/0 (for Jenkins)
9. 🗝️ Select or create a key pair.
10. 🚀 Launch your instance.

## 🖼️ EC2 Instance Launch Screenshot

![EC2 Instance Screenshot](https://github.com/Naveen15github/jenkins-on-aws-ec2/blob/f0586f43bec60bedc0266a8aadea74f8556f9e9d/Screenshot%20(21).png)

## 🔒 Add Port 8080 to Security Group

To access Jenkins, you need to allow inbound traffic on port 8080 in your EC2 Security Group.

![Add Port 8080 to Security Group Screenshot](https://github.com/Naveen15github/jenkins-on-aws-ec2/blob/f0586f43bec60bedc0266a8aadea74f8556f9e9d/Screenshot%20(22).png
)

### 2️⃣ Connect to Your EC2 Instance

Use your SSH key to connect:

```bash
ssh -i /path/to/your-key.pem ec2-user@<EC2-Public-IP>
# For Ubuntu: ssh -i /path/to/your-key.pem ubuntu@<EC2-Public-IP>
```

### 3️⃣ Install Jenkins

#### 🟡 On Amazon Linux 2:

```bash
sudo yum update -y
sudo amazon-linux-extras install java-openjdk11 -y
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

#### 🟠 On Ubuntu:

```bash
sudo apt update
sudo apt install openjdk-11-jre -y
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### 4️⃣ Access Jenkins Web UI

1. 🌐 Open `http://<EC2-Public-IP>:8080` in your browser.
2. 🔑 Retrieve the admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

3. ✨ Enter the password in the Jenkins setup page.

## 📸 Jenkins Setup Screenshot

![Jenkins Setup Screenshot](https://github.com/Naveen15github/jenkins-on-aws-ec2/blob/f0586f43bec60bedc0266a8aadea74f8556f9e9d/Screenshot%20(18).png)(https://github.com/Naveen15github/jenkins-on-aws-ec2/blob/f0586f43bec60bedc0266a8aadea74f8556f9e9d/Screenshot%20(20).png)

### 5️⃣ Initial Jenkins Setup

- 📦 Install suggested plugins as prompted.
- 🧑‍💻 Create your first admin user.
- 🤖 Start creating and configuring pipelines!

## 🛡️ Security Tips

- 🔐 Restrict port 8080 to trusted IPs.
- 🛡️ Use HTTPS for Jenkins (setup a reverse proxy if needed).
- 🔄 Regularly update Jenkins and plugins.

## 🧹 Cleanup

Terminate your EC2 instance when you’re done to avoid charges.
