# AWS Analytics Workshop
This README guides participants through setting up a unified analytics environment using Amazon SageMaker Studio and visualizing data in Amazon QuickSight.

AWS Region for this workshop: **ap-southeast-1 (Singapore Region)**

## Workshop Modules
1. **Setting Up SageMaker Unified Studio**
2. **Uploading Datasets to Unified Studio** - Learn how to import your CSV files into SageMaker
3. **Setting up ETL Pipeline in Unified Studio** - Learn how to use Visual ETL Flows and Juypter Notebooks to process your data
4. **Loading Data to Data Warehouse in Unified Studio** - Loading data to Redshift in SageMaker Unified Studio
5. **Visualization with QuickSight** - Create interactive dashboards with datasets from Redshift

## Setup Instructions

### IAM Identity Center Configuration

Set up AWS IAM Identity Center (successor to AWS Single Sign-On):

1. Navigate to the IAM Identity Center console
2. Click Enable
3. Create IAM Group > anycompany-admin
4. Create IAM Group > anycompany-salesmarketing
5. Create IAM User > dg-corp-admin. Add this user under anycompany-admin group.
7. Create IAM User > dg-business-analyst. Add this user under anycompany-salesmarketing group.

### Sign up for QuickSight
1. Navigate to QuickSight console and sign up for QuickSight
2. Authentication Method > Use AWS IAM Identity Center
3. QuickSight region > ap-southeast-1
4. QuickSight account name > eg. quicksight-21072025
5. For assigning permissions, click show more. Add anycompany-admin under Admin Pro group. Add anycompany-salesmarketing under Reader Pro group.
6. Uncheck "Add Pixel-Perfect Reports"

### Create a VPC
1. Resources to create: VPC and more
2. Name the VPC: sagemaker-workshop
3. Number of AZs: 3 public subnets, 3 private subnets
4. NAT Gateway: In 1 AZ
5. VPC Endpoints: None
6. S3 Gateway Endpoint: 1

### Create Amazon SageMaker Studio Domain
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
12. User Management > Add User: dg-business-analyst

### Enable QuickSight Blueprint in SageMaker Unified Studio
1. Access the SageMaker Unified Studio console
2. Click Blueprints > Select QuickSight > Click Enable
3. Leave the others as default
4. Click Enable blueprint
5. Return back to SageMaker Unified Studio Console. Select Project Profiles > Click All Capabilities
<img width="1488" height="329" alt="image" src="https://github.com/user-attachments/assets/738875b9-d5b4-4c17-9704-98158b87ab0a" />
7. Under Blueprint deployment settings > Add blueprint deployment settings
<img width="1475" height="619" alt="image" src="https://github.com/user-attachments/assets/50fe35ea-a79f-4365-911b-92aff807acec" />
9. Blueprint deployment settings name: quicksight-blueprint
10. Blueprint: Select QuickSight
11. Click Add blueprint deployment settings

### Download Workshop Data
Download the required CSV dataset files from the following links:
- [retail_sales_performance.csv](https://redshift-demos.s3.us-east-1.amazonaws.com/data/salesmarketing/lakehouse/retail_sales_performance.csv)
- [store_details.csv](https://redshift-demos.s3.us-east-1.amazonaws.com/data/salesmarketing/lakehouse/store_details.csv)

### Opening SageMaker Unified Studio
1. Select Open Unified Studio from your SageMaker domain created earlier
2. Login with SSO using dg-corp-admin
3. Click Create Project
4. Project Name: sagemaker-workshop-project
5. Project Profile: All capabilities
6. Click Next
7. Lakehouse Database: Change name to "lakehouse_db"
8. QuickSight > Restrictured Folder Name: quicksight-sagemaker-folder
9. Continue > Create Project

### Importing Datasets to SageMaker Unified Studio
1. Select Data > Click on the "+" button to add datasets to SageMaker Lakehouse > Select "Create Table"
2. <img width="1499" height="771" alt="image" src="https://github.com/user-attachments/assets/588b4377-b7fb-4aa2-9cca-375e55c938a8" />
3. Add data by uploading the retail_sales_perfomance.csv file
4. <img width="841" height="758" alt="image" src="https://github.com/user-attachments/assets/a312ac16-3580-4d0e-bcc1-8997e96b164a" />
5. Repeat the same steps to upload store_details.csv file
6. Once the datasets are imported, try to query them using Redshift or Athena
7. <img width="1493" height="531" alt="image" src="https://github.com/user-attachments/assets/4852cada-5e8f-49e0-8dc1-6b79b74b664f" />
8. <img width="1501" height="665" alt="image" src="https://github.com/user-attachments/assets/a56edc62-462e-4b93-aa03-6245f513d4ad" />

### Add Federated Redshift Catalog 
1. Data > "+" Button > Add connection
2. Select Amazon Redshift
4. Name: redshift-federated
5. Host name: eg. redshift-serverless-workgroup-<xxx>.<AWSACCOUNTID>.ap-southeast-1.redshift-serverless.amazonaws.com
6. Port: 5439
7. Database: dev
8. Authenication: AWS Secrets Manager > Select the secret pre-created for you
9. Click Add Data

### Create a target table in Redshift
We will set up an ETL pipeline later, we will first create the schema for the target table in Redshift
1. Build > Query Editor
2. Select Redshift connection, dev database, project schema. Click choose
3. <img width="1488" height="518" alt="image" src="https://github.com/user-attachments/assets/37f5eceb-46d6-4980-98b9-60f4860786e0" />
4. Enter this SQL query to create the table schema for the transformed table
```sql
CREATE TABLE project.combined_sales_store (
    date DATE,
    store_id VARCHAR(20),
    product_id VARCHAR(20),
    sales_amount DECIMAL(10,2),
    units_sold INTEGER,
    store_name VARCHAR(100),
    store_city VARCHAR(50),
    store_state CHAR(2),
    store_zip VARCHAR(5)
);
```

### Transforming Data in SageMaker Unified Studio (Visual ETL and Juypter Notebooks)
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
6. Add Rename node. Rename stored_id to stored_id_performance. 
7. Select "SageMaker Lakehouse" as data source
    - Catalog: AwsDataCatalog
    - Database: lakehouse_db_***
    - Table: store_details
    - Click "Update Node"
    - Rename the node to store_details
8. Add Rename node. Rename stored_id to stored_id_details.
9. Add Join node. Left Data Source is stored_id_performance, Right Data Source is stored_id_details.
10. Add Drop Column node. Drop stored_id_details, store_open_date, store_closed_date.
11. Add Change Column node to change some data types. Change date column from string to date data type.
12. Add Target > Redshift.
    - Name: redshift-federated
    - Schema: project
    - Table: combined_sales_store
    - Mode: Overwrite
    - Click Update Node
13. <img width="1502" height="732" alt="image" src="https://github.com/user-attachments/assets/3cc35c6b-bd48-43da-afab-9a341ba9c564" />
14. Click Update Project. Edit the job name. Select G.4X for instance type.
15. Click Update.

### Run ETL Job in Visual ETL Flows
1. Select your ETL flow > Click Run
2. Click "View Runs" to see the progress of your ETL runs.
3. <img width="1027" height="537" alt="image" src="https://github.com/user-attachments/assets/6f297198-d8d9-4cb8-a672-a4a52fff61c8" />
4. It should show succeeded. If failed, click the error message to troubleshoot.

### Querying Target Table in Amazon Redshift
1. Build > Query Editor
2. Query the target table in Redshift
```sql
SELECT * FROM project.combined_sales_store LIMIT 10;
```

### Adding Redshift Dataset to Project Catalog in SageMaker Unified Studio
1. Project Catalog > Data Sources > Create Data Source
2. Data Source Name: data_source_combined_sales_store
3. Data Source Type: Redshift
4. Connection: project.redshift
5. Data Selection > Schema: project
6. Data Selection > Table: combined_sales_store
7. Click Next
8. Publish assets to catalog: Yes
9. Click Next
10. Run preference: Run on demand
11. Click Create

### Adding Assets in Unified Studio to QuickSight
1. Project Catalog > Assets
2. Click on "combined_sales_store" asset
3. Select Actions > Open in QuickSight. SageMaker Unified Studio creates a QuickSight dataset and organizes it in a secured folder accessible only to members within the project.
4. <img width="1501" height="385" alt="image" src="https://github.com/user-attachments/assets/08d23d8f-fdc8-41c9-a687-9869522758ba" />
5. Create an analysis in QuickSight with the imported dataset

### Creating QuickSight Visualizations
1. Create a line chart:
    - X Axis: date (Quarter)
    - Value: sales_amount
2. Create a line chart:
    - X Axis: date (Quarter)
    - Value: units_sold
3. Create a horizontal stacked bar chart:
    - Y Axis: store_city
    - Value: sales_amount
    - Group: store_state
2. Create a horizontal stacked bar chart:
    - Y Axis: store_city
    - Value: units_sold
    - Group: product_id


## Troubleshooting
- **Permission Issues**: Verify IAM permissions are correctly assigned
- **Domain Creation Failures**: Check VPC settings and service quotas
- **QuickSight Integration Problems**: Ensure subscription is active and properly configured

## Clean Up
To prevent ongoing AWS charges after completing the workshop, it's important to clean up and delete the resources you've created. Here's a suggested cleanup process:
1. Delete QuickSight resources:
   - Delete any analyses and dashboards created in QuickSight
   - Delete the QuickSight dataset created from the Redshift table
   - Unsubscribe from QuickSight if you no longer need it

2. Clean up SageMaker Unified Studio:
   - Delete the project created (sagemaker-workshop-project)
   - Delete any notebooks or other files created in the Studio environment
   - Delete the SageMaker Studio Domain (sagemaker-workshop-domain)

3. Delete Redshift resources:
   - Delete the table created in Redshift (project.combined_sales_store)
   - If a Redshift cluster was created specifically for this workshop, delete it

4. Clean up S3:
   - Delete any S3 buckets created for this workshop, including those automatically created by SageMaker

5. Delete the VPC:
   - Delete the VPC endpoints
   - Delete the NAT Gateway
   - Delete the VPC (sagemaker-workshop)

6. IAM cleanup:
   - Remove the IAM users created (dg-corp-admin, dg-business-analyst)
   - Delete the IAM groups created (anycompany-admin, anycompany-salesmarketing)
   - Review and delete any IAM roles created for this workshop

7. Disable IAM Identity Center:
   - If you enabled IAM Identity Center solely for this workshop, you may want to disable it

Remember to be cautious when deleting resources, as this process is irreversible. Always double-check before deleting any resource to ensure you're not removing anything critical to other projects or applications.

## Support
For questions or issues during the workshop, please contact the workshop facilitators or open an issue in this repository.
