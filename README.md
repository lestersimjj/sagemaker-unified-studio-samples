# AWS Analytics Workshop
This README guides participants through setting up a unified analytics environment using Amazon SageMaker Studio and visualizing data in Amazon QuickSight.

AWS Region for this workshop: **ap-southeast-1 (Singapore Region)**
## Setup Instructions

### 1. IAM Identity Center Configuration

Set up AWS IAM Identity Center (successor to AWS Single Sign-On):

1. Navigate to the IAM Identity Center console
2. Click Enable
3. Create IAM Group > anycompany-admin
4. Create IAM Group > anycompany-salesmarketing
5. Create IAM User > dg-corp-admin. Add this user under anycompany-admin group.
7. Create IAM User > dg-business-analyst. Add this user under anycompany-salesmarketing group.

### 2. Sign up for QuickSight
1. Navigate to QuickSight console and sign up for QuickSight
2. Authentication Method > Use AWS IAM Identity Center
3. QuickSight region > ap-southeast-1
4. QuickSight account name > eg. quicksight-21072025
5. For assigning permissions, click show more. Add anycompany-admin under Admin Pro group. Add anycompany-salesmarketing under Reader Pro group.
6. Uncheck "Add Pixel-Perfect Reports"

### 3. Create a VPC
1. Resources to create: VPC and more
2. Name the VPC: sagemaker-workshop
3. Number of AZs: 3 public subnets, 3 private subnets
4. NAT Gateway: In 1 AZ
5. VPC Endpoints: None
6. S3 Gateway Endpoint: 1

### 3. Create Amazon SageMaker Studio Domain
1. Navigate to the Amazon SageMaker console
2. Select "Create a Unified Studio domain"
3. Quick setup
4. Expand Quick setup settings
5. Domain Name: sagemaker-workshop-domain
6. VPC: sagemaker-workshop
7. Subnet: Select all 3 private subnets
8. Leave the others as default
9. Click continue
10. Create IAM Identity Center User: dg-corp-admin
11. Click Create Domain

### 3. Enable QuickSight Blueprint in SageMaker Unified Studio
1. Access the SageMaker Unified Studio console
2. Click Blueprints > Select QuickSight > Click Enable
3. Leave the others as default
4. Click Enable blueprint
5. Return back to SageMaker Unified Studio Console. Select Project Profiles > Click All Capabilities
6. <img width="1488" height="329" alt="image" src="https://github.com/user-attachments/assets/738875b9-d5b4-4c17-9704-98158b87ab0a" />
7. Under Blueprint deployment settings > Add blueprint deployment settings
8. <img width="1475" height="619" alt="image" src="https://github.com/user-attachments/assets/50fe35ea-a79f-4365-911b-92aff807acec" />
9. Blueprint deployment settings name: quicksight-blueprint
10. Blueprint: Select QuickSight
11. Click Add blueprint deployment settings


### 4. Download Workshop Data

Download the required CSV dataset files from the following links:
- [Sample Dataset 1](#) (placeholder link)
- [Sample Dataset 2](#) (placeholder link)

## Workshop Modules

1. **Data Ingestion** - Learn how to import your CSV files into SageMaker
2. **Data Exploration and Preparation** - Use SageMaker Data Wrangler to clean and transform your data
3. **Machine Learning** - Build and deploy a simple ML model
4. **Visualization with QuickSight** - Create interactive dashboards

## Troubleshooting

- **Permission Issues**: Verify IAM permissions are correctly assigned
- **Domain Creation Failures**: Check VPC settings and service quotas
- **QuickSight Integration Problems**: Ensure subscription is active and properly configured

## Clean Up

To avoid incurring unnecessary charges after completing the workshop:
1. Delete any deployed endpoints and models
2. Stop notebook instances
3. Delete Studio domain if no longer needed

## Support

For questions or issues during the workshop, please contact the workshop facilitators or open an issue in this repository.
