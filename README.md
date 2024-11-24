Deploying Two EC2 Instances in a Custom VPC Using AWS CloudFormation
Introduction
This project contains an AWS CloudFormation template (Cloudformation.yaml) that automates the deployment of a custom Virtual Private Cloud (VPC) with public and private subnets and launches two EC2 instances within them. This setup demonstrates the use of infrastructure as code to provision AWS resources efficiently and reproducibly.

Table of Contents
Introduction
Prerequisites
Architecture Overview
Getting Started
Clone the Repository
Validate the Template (Optional)
Deploy the CloudFormation Stack
Access the EC2 Instances
Template Details
Parameters
Resources Created
Outputs
Cleanup
Contributing
License
Acknowledgments
Prerequisites
AWS Account: You must have an active AWS account.
IAM Permissions: Ensure you have permissions to create CloudFormation stacks and AWS resources (VPCs, subnets, EC2 instances, etc.).
AWS CLI Installed: For validating and deploying the template via command line (optional).
SSH Key Pair: An existing EC2 key pair in the AWS region where you plan to deploy the stack.
Text Editor: To view or modify the CloudFormation template (optional).
Architecture Overview
The CloudFormation template sets up the following architecture:

A custom VPC with a CIDR block of 10.0.0.0/16.
Public Subnet (10.0.1.0/24) in a specified Availability Zone.
Private Subnet (10.0.2.0/24) in the same Availability Zone.
An Internet Gateway attached to the VPC.
Route Tables:
Public Route Table with a route to the Internet Gateway.
Private Route Table without internet access.
Security Group allowing SSH (port 22) and HTTP (port 80) inbound traffic.
EC2 Instances:
One EC2 instance in the public subnet.
One EC2 instance in the private subnet.

Note: Replace the above line with an actual architecture diagram if available.

Getting Started
1. Clone the Repository
bash
Copy code
git clone https://github.com/jenniferasuwata/cloudformation_templates.git
cd cloudformation_templates
2. Validate the Template (Optional)
You can validate the CloudFormation template using the AWS CLI:

bash
Copy code
aws cloudformation validate-template \
    --template-body file://Cloudformation.yaml \
    --region your-region
Replace your-region with the AWS region you intend to use (e.g., eu-west-1).

3. Deploy the CloudFormation Stack
Via AWS Management Console
Sign In: Log in to the AWS Management Console.
Navigate to CloudFormation: Search for CloudFormation in the services menu.
Create Stack:
Click on Create stack and select With new resources (standard).
Upload Template:
Under Specify template, choose Upload a template file.
Click Choose file and select Cloudformation.yaml from your cloned repository.
Configure Stack Details:
Stack Name: Enter a unique name for your stack (e.g., CustomVPCStack).
Parameters: Provide the required parameters (e.g., your EC2 Key Pair name).
Configure Stack Options:
Optionally add tags or configure advanced options.
Review and Create:
Review your settings, acknowledge any required capabilities, and click Create stack.
Via AWS CLI
bash
Copy code
aws cloudformation create-stack \
    --stack-name CustomVPCStack \
    --template-body file://Cloudformation.yaml \
    --parameters ParameterKey=KeyName,ParameterValue=your-key-pair-name \
    --region your-region \
    --capabilities CAPABILITY_NAMED_IAM
Replace CustomVPCStack, your-key-pair-name, and your-region with your desired stack name, EC2 key pair name, and AWS region, respectively.

4. Access the EC2 Instances
Public EC2 Instance:
Retrieve the public IP from the EC2 console or CloudFormation outputs.
Connect via SSH:
bash
Copy code
ssh -i /path/to/your-key-pair.pem ec2-user@public-ip-address
Private EC2 Instance:
Since it doesn't have a public IP, you can SSH into it from the public EC2 instance:
bash
Copy code
ssh ec2-user@private-ip-address
Ensure agent forwarding is enabled if using key pairs.
Template Details
Parameters
KeyName: (Required) Name of an existing EC2 KeyPair to enable SSH access to the instances.
Resources Created
VPC: Custom VPC named CloudformationVPC.
Subnets:
PublicSubnet in eu-west-1b Availability Zone.
PrivateSubnet in eu-west-1b Availability Zone.
Route Tables:
PublicRouteTable associated with PublicSubnet.
PrivateRouteTable associated with PrivateSubnet.
Internet Gateway: InternetGateway attached to the VPC.
Security Group: InstanceSecurityGroup allowing SSH and HTTP access.
EC2 Instances:
PublicEC2Instance in the public subnet.
PrivateEC2Instance in the private subnet.
Outputs
PublicInstancePublicIP: The public IP address of the public EC2 instance.
PrivateInstanceID: The instance ID of the private EC2 instance.
Note: You can modify the template to include these outputs if not already present.

Cleanup
To avoid incurring unnecessary charges, delete the CloudFormation stack when you're done:

Via AWS Management Console
Navigate to the CloudFormation service.
Select your stack (CustomVPCStack).
Click on Delete and confirm the deletion.
Via AWS CLI
bash
Copy code
aws cloudformation delete-stack --stack-name CustomVPCStack --region your-region
Contributing
Contributions are welcome! If you have suggestions for improving the template or documentation, please open an issue or submit a pull request.

License
This project is licensed under the MIT License. See the LICENSE file for details.

Acknowledgments
Inspired by AWS CloudFormation best practices and examples.
Thanks to the AWS community and documentation for guidance.
Feel free to customize this README to better suit your project's specifics or to add more detailed instructions.
