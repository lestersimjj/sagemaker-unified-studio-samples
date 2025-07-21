<img width="1501" height="665" alt="image" src="https://github.com/user-attachments/assets/55722003-d425-4290-9da0-98958def17bb" /># AWS Analytics Workshop
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
- [retail_sales_performance.csv](https://redshift-demos.s3.us-east-1.amazonaws.com/data/salesmarketing/lakehouse/retail_sales_performance.csv)
- [store_details.csv](https://redshift-demos.s3.us-east-1.amazonaws.com/data/salesmarketing/lakehouse/store_details.csv)

### 5. Opening SageMaker Unified Studio
1. Select Open Unified Studio from your SageMaker domain created earlier
2. Login with SSO using dg-corp-admin
3. Click Create Project
4. Project Name: sagemaker-workshop-project
5. Project Profile: All capabilities
6. Click Next
7. Lakehouse Database: Change name to "lakehouse_db"
8. QuickSight > Restrictured Folder Name: quicksight-sagemaker-folder
9. Continue > Create Project

### 6. Importing Datasets to SageMaker Unified Studio
1. Select Data > Click on the "+" button to add datasets to SageMaker Lakehouse > Select "Create Table"
2. <img width="1499" height="771" alt="image" src="https://github.com/user-attachments/assets/588b4377-b7fb-4aa2-9cca-375e55c938a8" />
3. Add data by uploading the retail_sales_perfomance.csv file
4. <img width="841" height="758" alt="image" src="https://github.com/user-attachments/assets/a312ac16-3580-4d0e-bcc1-8997e96b164a" />
5. Repeat the same steps to upload store_details.csv file
6. Once the datasets are imported, try to query them using Redshift or Athena
7. <img width="1493" height="531" alt="image" src="https://github.com/user-attachments/assets/4852cada-5e8f-49e0-8dc1-6b79b74b664f" />
8. <img width="1501" height="665" alt="image" src="https://github.com/user-attachments/assets/a56edc62-462e-4b93-aa03-6245f513d4ad" />

### 7. Transforming Data in SageMaker Unified Studio (Visual ETL and Juypter Notebooks)
1. Select Visual ETL Flows in the topbar
2. <img width="1037" height="237" alt="image" src="https://github.com/user-attachments/assets/18fec5ad-2a1b-4b59-9166-f6cab3efcbe8" />
3. Create visual ETL flow
4. Select "Data managed using full-data access"
5. Select "SageMaker Lakehouse" as data source
    - Catalog: AwsDataCatalog
    - Database: lakehouse_db_***
    - Table: retail_sales_performance
    - Click "Update Node"
    - Rename the node to retail_sales_performance
6. Select "SageMaker Lakehouse" as data source
    - Catalog: AwsDataCatalog
    - Database: lakehouse_db_***
    - Table: store_details
    - Click "Update Node"
    - Rename the node to store_details



### 8. Querying Transformed Data using Amazon Redshift

### 9. 

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
