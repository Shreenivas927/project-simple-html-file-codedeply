Step 1: Create required IAM Roles
Create role for EC2 instance with permissions for CodeDeploy and S3 access to download the code.
Create a role for CodeDeploy service with permissions to access S3 and basic CodeBuild functionalities.
Step 2: Create S3 bucket to store source code and build artifacts
Step 3: Create Code Build project
Here we will create a Build Project that will take source code from GitHub repository, and authenticate AWS CodeBuild to use code from our GitHub account.
Step 4: Create Deployment Application
We will create a CodeDeploy application that will deploy the applications to EC2 instance.
Step 5: Launch an EC2 Instance to host the application
Launch an EC2 instance that will have the custom role attached to it created in Step 1 and with proper tag so it can be used later.
Attach the security group with port 22 open for ssh and port 80 open to access the web application.
SSH to EC2 instance and install the CodeDeploy agent so CodeDeploy can deploy the application.
Commands to install CodeDeploy agent

# Installing CodeDeploy Agent
sudo yum update
sudo yum install ruby
# Download the agent (replace the region)
wget https://aws-codedeploy-eu-west-3.s3.eu-west-3.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent status
Step 6: Create Deployment Group
In the CodeDeploy application created earlier in Step 4, create Deployment Group and mention the tag of EC2 instance created earlier to deploy application on instances having that specific tag.
Step 7: Create a CodePipeline
In Stage 1 we will select GitHub as cource code repository and choose the artifacts to be stored at the S3 bucket created earlier.
In Build stage we will choose the Build Project created by us.
In the Deploy stage select the deployment application and group created in the previous steps.
Step 8: Update the IAM Role for CodePipeline
Here we will update the IAM role created by CodePipeline and attach the access policy granting full access of S3 to CodePipeline.