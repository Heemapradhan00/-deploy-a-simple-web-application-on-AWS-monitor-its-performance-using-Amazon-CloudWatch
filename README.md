## Steps to Implement

### 1. **Create an EC2 Instance**
- Log in to the **AWS Management Console**.
- Launch a new EC2 instance with the following configuration:
  - **AMI**: Amazon Linux or Ubuntu
  - **Instance Type**: t2.micro (Free Tier eligible)
  - **Storage**: Default 8GB
  - Configure the Security Group to allow:
    - HTTP (Port 80)
    - SSH (Port 22)
- Download the **key pair file** (.pem) and save it securely.

### 2. **Deploy a Simple Web Application**
- SSH into the EC2 instance:
  
  ssh -i your-key-file.pem ec2-user@your-ec2-public-ip
 
- Install and configure a basic web server:

  sudo yum update -y
  sudo yum install httpd -y
  sudo systemctl start httpd
  sudo systemctl enable httpd
  echo "Welcome to My Web Application!" > /var/www/html/index.html
 
- Verify the web application by accessing the public IP of your EC2 instance in a browser.

---

### 3. **Configure Amazon CloudWatch**
- Install the CloudWatch Agent:
 
  sudo yum install amazon-cloudwatch-agent -y
 
- Create a configuration file for the CloudWatch Agent:
 
  sudo nano /opt/aws/amazon-cloudwatch-agent/bin/config.json
 
  Example configuration:
  ```json
  {
    "metrics": {
      "metrics_collected": {
        "cpu": {
          "measurement": ["cpu_usage_idle", "cpu_usage_iowait"],
          "metrics_collection_interval": 60
        },
        "mem": {
          "measurement": ["mem_used_percent"],
          "metrics_collection_interval": 60
        }
      }
    }
  }
  
- Start the CloudWatch Agent:

  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a start -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s

### 4. **Create a CloudWatch Dashboard**
- Open the CloudWatch Dashboard in the AWS Console.
- Create a new dashboard and add widgets for key metrics, such as:
  - **CPU Utilization**
  - **Memory Usage**
  - **Disk Usage**
- Save and monitor the metrics in real time.
  ![image](https://github.com/user-attachments/assets/b0232032-e4a8-4296-8bb2-ff47e1db3296)


## Resources
- [AWS Management Console](https://aws.amazon.com/)  
- Refer to the official AWS documentation for additional details on EC2 and CloudWatch.
