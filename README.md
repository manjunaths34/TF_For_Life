# TF_For_Life

# Introduction:
- Overview of TF & Certification
- About the Course & Resources
- Document - Code Repository
	- https://github.com/zealvora/terraform-beginner-to-advanced-resource
- Our Community
- Basics of IAC (Infra as Code)
- Choosing Right IAC Tool
- Central PPT Notes (There is a pdf which we can download)

# Getting Started & Setting Up Labs:
- Installation process of terraform
	- Have installed it on my personal laptop, should use it day by day and do more hands-on (CONSISTENTLY)
	- In Advanced Settings --> Environment variables --> Path Variable , add the folder path where TF Binary is moved to (Created Binaries folder in C folder and moved TF Binary there)
- Document - TF Downloads Page (https://www.terraform.io/downloads)
- Installing TF - MacOS & Linux Users
- Choosing Right IDE For TF (VSS Is best along with extensions it gives)
- Install & Setup Source Code Editor
- VSS Extensions
- Sample Code - Extension Test
- Setting up AWS Account

# Deploying Infra with Terraform:
- Authentication & Authorization
	- For AWS we need access & secret access keys to setup the authentication part
	- He created a user and for that user , he created access & secret access key to setup this user in TF and work with AWS
- Create User for AWS Account
- Launch First Virtual Machine through TF
- TF Code - First EC2 instance
- Imp Security Pointer
- Resource & Providers
- Provider Tiers
- Create Github repo through TF
	- For any thing use the documentation in-depth and it is quite easy.
	- Here for github repo to be created, use the token from github as well
- TF Destroy
	- terraform destroy -target aws_instance.myec2
	- If you still have the infra related code in the .tf file, TF will still plan and create that infra for you, so , better to remove the code as well (or) comment the code
- Understanding TF State Files
	- terraform.tfstate (Donot touch these files ideally and also make sure you have a backup of these tfstate file)
- Understanding Desired & Current States
	- Terraform always tried to match your infra to desired state , meaning to be in-sync with what is present in your code
	- Even if you manually change something on AWS/Azure consoles , but, if you do terraform plan in the folder terraform will try and bring the infra back to what it is in code written (in your .tf file)
- Challenges with the current state on computed values
	- Zeal showed that he changed the security group for an existing EC2 from AWS console and tried terraform plan. Terraform didn't show any changes it needs to bring in the desired state. This is because , we didn't add any security group related code in the .tf file and hence terraform doesn't think it needs to change anything to its desired state. This is the challenge which desired state and current state brings in and hence we should be very in-depth in writing the code in our .tf files.
- TF Provider Versioning
- TF Refresh
	- Donot use terraform refresh much live in PROD
- AWS Provider - Authentication Configuration
	- aws-provider-config.tf (zeal added the access & secret access key's here along with a demouser creation)
	- Ohter way is to install aws cli and use aws configure and provide access & secret key's so that they are saved in a safe location and not hard-coded in the .tf code files
	- terraform apply -auto-approve

# Read, Generate, Modify Configurations:
- Lecture Format - TF Course
	- Try & have seperate folders for each category of code you write
- Learning Scope - AWS Services for TF
- Basics of Firewalls in AWS
	- netstat -ntlp
- Security Group practical
	- very basic SG group creation is done
- Creating Firewall rules using TF
	- Showed how to create the SG and add inbound & outbound rules into it.
- Dealing with Documentation Code Updates
	- Older approach will also work, just FYI.
- Creating Elastic IP with TF
	- Easy to create the EIP, no need to assign to any instance while creating. Can just create it. But make sure to destroy it if not used.
- Basic of Attributes
	- For any resource once created, Attributes are added in the statefile.
- Cross Resource Attribute References
 - Scenarios where resource 2 might be dependent on some value of resource 1
 - Something like ${aws_eip.lb.public_ip}/32 can be used to put it in the ingress rule in SG inbound rules addition
- Cross Resource Attribute References - Practical(NEW)
 - Something like ${aws_eip.lb.public_ip}/32 can be used to put it in the ingress rule in SG inbound rules addition

# Output Values:
- Makes information about infra available on the command line , and can expose information for other terraform config's to use




























