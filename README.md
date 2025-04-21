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
  output "public_ip"
  { value = aws_eip.lb.public_ip
  } 
This above code will give you the public ip value when you do the terraform apply.
- value = aws_eip.lb
- If we use value just like above it will show attribute values of that particular resource fully.

# Overview of TF Variables
- Create a central-file.tf (or) variables.tf and put your variables and values here something like below
  variable var_ip
  {
  	default = 10.0.06/32
  }

  variable var_port
  {
  	default = 8080
  }

- Use these variables when you are writing your code to create the ingress rules, something like below
  resource <<ingress>> "allow_tls_ipv4"
  {
	security_group_id	= aws_security_group.allow_tls.id
	cidr_ipv4 		= var.var_ip
	port			= var.var_port
  }

# Terraform Variables - Practical
- Same as above, but, he showed by creating thats it

# Variable Definitions File (TFVars)
- Main TF Config File, variables.tf, terraform.tfvars file (Define values associated with your vairables here) , this is the structure you can ideally have
- terraform plan --var-file="dev.tfvars" (or) "prod.tfvars"
- if default value is added and no data in .tfvars then default value is picked
- if you have default value and value in .tfvars both then value from .tfvars will only be picked up and not the other one
- if you dont name the file as terraform.tfvars then we are asked the variable value when we are planning the terraform plan
- if we do not specify any specific .tfvars file then we can do like --var-file prod.tfvars so that variable values are picked from that particular tfvars file and plan is run smoothly

# Approaches for Variable Assignment (NEW)
- If you have not defined a value for the variable. TF will ask you to input the value in CLI Prompt when you run the TF plan/apply operation both.
- Variable Defaults, Variable Definition File (*.tfvars), Env Variables, Setting variables in Command Line **(4 Methods to define/provide values to the variables)**
- Example to provide variable value as part of command Line
  terraform plan -var="instance_type=m5.large"
- Example to provide variable value as part of ENV Variable (There is a specific variable format which you should use)
  TF_VAR_instance_type = m5.large **(You should addd this in system properties and not just on command line)**
  echo %TF_VAR_instance_type% will show you the value you have set and then you can use this variable

# Setting ENV Variable in Linux
- export TF_VAR_instance_type = t2.large
- echo $TF_VAR_instance_type

# Variable Definition Precedence
- Order of Variable Preference --> Env Variables, terraform.tfvars, terraform.tfvars.json, *.auto.tfvars, *.auto.tfvars.json, any -var & -var-file options in command line
- Which ever is used at last, that is the value it picks up and not the older one

# Data Types in TF
- variable username
  {
  	type = number
  }

# Data Type - List
- variable "my-list"
  {
  	type = list(number)
  	default = [1,2,3,4,5]
  }

# Data Type - Map
- Map is usually a key, value pair data type which we can use and can be used most of the times for tags
- variable "my-map"
  {
  	type = map
  	default = {
  		Name = "Alice"
  		Team = "Payments"
  	}
  }

  output "variable_value"
  {
  	value = var.my_map
  }

# Fetching Data from Maps and List in variable
- resource "aws_instance" "my-ec2"
  {
  	ami = "ami-123456"
  	instance_type = var.types["us-west-2"]
  	instance_type = var.list[1]
  }

  variable "list"
  {
	type = list
	default = ["m5.large","t2.medium","m5.xlarge"]
  }	

  vairable "types"
  {
	type = map
	default =
	{
		us-east-1 = "t2.micro"
  		us-west-2 = "t2.nano"
  		ap-south-1 = "t2.small"
	}
  }















