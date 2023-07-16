
​​# Three-Tier Architecture Deployment on AWS with Terraform
For this project, I created a three-tier architecture  consisting of a Web tier , Application tier and a Database tier. I used Terraform modules to make the process easily repeatable and reusable.  This deployment will create a scalable, secure and highly available infrastructure that separates the web, application, and database layers. Ensuring they are all communicating with each other. The architecture includes an Amazon Virtual Private Cloud (VPC), Elastic Load Balancer (ELB), Auto Scaling Group (ASG), and a Relational Database(RDS).
The Web tier will have a bastion host and NAT gateway provisioned in the public subnets. The bastion host will serve as our access point to the underlying infrastructure. The NAT Gateway will allow our private subnets to communicate with the internet  while maintaining  a level of security by hiding the private instances' private IP addresses from the public internet.
In the Application tier, we will create an internet facing load balancer to direct internet traffic to an autoscaling group in the private subnets, along with a backend autoscaling group for our backend application. We will create a script to install the apache webserver in the frontend, and a script to install Node.js in the backend.
In the Database tier, we will have another layer of private subnets hosting a MySQL database which will  eventually be accessed using Node.js..
 
[Architecture diagram here](../cloudgen-architecture)
 
 
I have provided a step-by-step guide to deploying this architecture on Amazon Web Services (AWS) using Terraform.

Please note that this guide assumes you have basic knowledge of AWS services and Terraform. If you're new to AWS or Terraform, it's recommended to familiarize yourself with these technologies before proceeding.
 
## Prerequisites
 
Before you begin, ensure that you have the following prerequisites:
 
1. AWS account credentials (access key ID and secret access key).
2. Terraform installed on your local machine. You can download Terraform from the official website: https://www.terraform.io/downloads.html.
3. Basic knowledge of AWS services such as EC2, VPC, ELB, ASG, and RDS.
4. Familiarity with the basics of Terraform, including how to write Terraform configuration files (`.tf`).
 
## Steps
 
Follow these step-by-step instructions to deploy a three-tier architecture on AWS using Terraform:
 
### Step 1: Clone the Repository
 
1. Open a terminal or command prompt on your local machine.
2. Clone the repository containing the Terraform configuration files:
   ```
   git clone https://github.com/your-repo-url.git
   ```
3. Change into the project directory:
   ```
   cd your-repo-directory
   ```
 
### Step 2: Configure AWS Credentials
 
1. Open the AWS Management Console in your web browser.
2. Navigate to the **IAM** service.
3. Create a new IAM user or use an existing one.
4. Assign the necessary permissions to the IAM user, such as `AmazonEC2FullAccess`, `AmazonRDSFullAccess`, `AmazonVPCFullAccess`, and `ElasticLoadBalancingFullAccess`.
5. Generate an access key ID and secret access key for the IAM user.
6. Configure the AWS CLI with the IAM user credentials using the following command:
   ```
   aws configure
   ```
   Enter the access key ID and secret access key when prompted, and optionally set the default region.
 
### Step 3: Configure Terraform Variables
 
1. Open the project directory in a text editor.
2. Locate the Terraform configuration file named `terraform.tfvars”. 
3. Modify the values of the variables according to your requirements.
   - `database_username`: Set the username for the database.
   - `database_password`: Set the password for the database.
   - `database_name`: Set the name of the database.
   - `instance_type`: Set the EC2 instance type for the web servers.
   - `desired_capacity`: Set the desired capacity for the Auto Scaling Group.
Do not forget to gitignore your .tfvars file 
 
### Step 4: Initialize Terraform
 
1. In the terminal or command prompt, navigate to the project directory., cd to the root directory ‘terraform’
2. Run the following command to fix any syntax issue
    ```
    terraform fmt
    ```
3. Run the following command to initialize Terraform and download the required providers:
   ```
   terraform init
   ```
 
### Step 5: Review and Validate the Configuration
 
1. Run the following command to review the changes that Terraform will make:
   ```
   terraform plan
   ```
   Review the output to ensure that the planned infrastructure matches your expectations.
 
### Step 6: Deploy the Infrastructure
 
1. Run the following command to deploy the infrastructure:
   ```
   terraform apply
   ```
   Terraform will show you a summary of the changes that will be made. Type `yes` to confirm and start the deployment.
 
2. Wait for Terraform to provision the infrastructure. This process may take several minutes.
 
### Step 7: Access the Application
 
1. After the deployment is complete, Terraform will output the DNS name of the ELB.
2. Copy the DNS name and paste it into your web browser.
3. If everything is set up correctly, you should see the application running.
 
### Step 8: Destroy the Infrastructure (Optional)
 
If you want to tear down the infrastructure and remove all resources created by Terraform, you can follow these steps:
 
1. In the terminal or command prompt, navigate to the project directory.
2. Run the following command to destroy the infrastructure:
   ```
   terraform destroy
   ```
   Type `yes` to confirm the destruction.
 
## Conclusion
 
Congratulations! You have successfully deployed a three-tier architecture on AWS using Terraform. This architecture provides a scalable and highly available infrastructure for your applications. Make sure to follow AWS best practices and security recommendations when deploying your production workloads.

##Testing
If you go into your AWS console, you should be able to see the VPC and subnets, internet gateway, route tables and associations, EC2 instances running in the proper locations, load balancers, and RDS database.
[vpc](../vpc-image)
[subnet](../subnet-image)
[ec2](../ec2-image)
[lb](../lb-image)
[tg](../tg-image)
[db](../db-image)
If we copy the load balancer endpoint we got from our Terraform output, and place it in the search bar, we will see the message we specified in our script for the Apache webserver.
If we refresh the page, we should see the IP address from the other instance in our frontend autoscaling group.
Make sure to test out the infrastructure. You will need to use the keypair to SSH into the bastion host. Locate the public IP address of your Bastion Instance in the console:







