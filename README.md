------------------------------------------------------------------------------------------------------------------
Deployment instructions for Servian techchallenge application in AWS cloud environment.
------------------------------------------------------------------------------------------------------------------

******************************************************************************************************************
				          General Instructions
******************************************************************************************************************
1. gtdapplicationstack.yml    - Template for deploying infrastrctures and workloads.
2. gtdapplicationpipeline.yml - Template for creating CI/CD codepipline for deploying GTD application.
3. GTD_Architecture.pdf       - High level architecture diagram.
4. README.md                  - File for deployment instructions.



******************************************************************************************************************
                                             Prerequisites
******************************************************************************************************************

1. Make sure that below CIDR block is available for deploying network components such as VPC, Subnets, IGW, 
   NAT Gateway inside AWS cloud.
	VPC CIDR - 10.0.0.0/16
        Public Subnet-A - 10.0.1.0/24
        Public Subnet-B - 10.0.2.0/24
        Application Subnet-A - 10.0.3.0/24
        Application Subnet-B - 10.0.4.0/24
        Database Subnet-A - 10.0.5.0/24
        Database Subnet-B - 10.0.6.0/24

******************************************************************************************************************
                                           Deployment Steps
******************************************************************************************************************
Step 1:
	Clone the "serviangtdapplication" repository from github. Following files will be downloaded.
		1. gtdapplicationstack.yml    
		2. gtdapplicationpipeline.yml 
		3. GTD_Architecture.pdf       
		4. README.md                  

Step 2:
	Open the AWS console and deploy the GTD application CI/CD pipeline stack using the below file. This will create 
	a pipeline consist of CodeCommit repository (GTDApplicationrepo) and CodeDeploy stages as shown in GTD_Architeture 
	diagram along with roles required.
	for CodePipeline, CloudFormation .
		1. gtdapplicationpipeline.yml

Step 3:
	Commit the below file in the newly created CodeCommit repository (GTDApplicationrepo). It will trigger the 
	pipeline to deploy the resouces required for gtd application.
		1. gtdapplicationstack.yml

Step 4:
	Open CloudFormation service and find the stack "gtdapplicationstack" and navigate to ouputs section to get the 
	Application laod balancers DNS to access the GTD Application.
******************************************************************************************************************
                                           Improvements 
******************************************************************************************************************
Security:
	ALB can be integrate with WAF.
	ALB can be integrated with ACM for SSL terminations and secure connections.
	KMS encryption can be used for all the data encryptions in CodeCommit, RDS, Cloudwatch logs.

Architecture:
	Aurora for PostgreSQL or DynamoDB can be used for performance improvement.
	ECS EC2 can be used for more control on the infrastructure but need to maintain server capacity.
	Another Pipeline can be added to create the image using CodeBuild and upload it into ECR which will trigger the application
	deployment pipeline.
Logs:
	VPC flow logs can be enabled for VPC, Subnets to troubleshoot the operational issues.
	ALB access logs canbe enabled for monitor the traffic pattern.