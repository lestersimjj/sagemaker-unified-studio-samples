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
Set up AWS IAM Identity Center:
1. Navigate to the IAM Identity Center console
   <img width="1211" height="175" alt="image" src="https://github.com/user-attachments/assets/6341e0b0-22ec-4a3b-a0f3-628c6b7d192d" />
3. Click Enable
   <img width="1055" height="368" alt="image" src="https://github.com/user-attachments/assets/bc560a00-f36e-4b9d-9637-9dbe98a7f929" />
   <img width="1485" height="648" alt="image" src="https://github.com/user-attachments/assets/6bb55aba-1eae-42ad-83cc-28e09cbf496b" />
4. Create IAM Group > anycompany-admin
   <img width="1498" height="304" alt="image" src="https://github.com/user-attachments/assets/3f958d1d-dac3-490f-857b-91eeb967fe41" />
   <img width="1490" height="671" alt="image" src="https://github.com/user-attachments/assets/020905f9-5bc4-4380-98ed-d80098315ae1" />
5. Create another IAM Group > anycompany-salesmarketing
6. Create IAM User
    - Username: dg-corp-admin
    - Password: "Generate a one-time password that you can share with this user"
    - Email: Enter your email
    - First/Last Name: Enter your name
    - Groups: anycompany-admin
    <img width="1073" height="684" alt="image" src="https://github.com/user-attachments/assets/4594e757-7bed-450a-b0b1-ac62fbe16aba" />
    <img width="1489" height="317" alt="image" src="https://github.com/user-attachments/assets/1fb3a17a-79f9-451d-bfba-2868898e14b6" />

7. Copy the one-time password and paste in your local notepad as we'll need to login in subsequent steps.
   <img width="585" height="461" alt="image" src="https://github.com/user-attachments/assets/ab57a94e-148d-4bb4-bc12-7daafaddcd4a" />

8. Create another IAM User
    - Username: dg-business-analyst
    - Password: "Generate a one-time password that you can share with this user"
    - Email: Enter your email
    - First/Last Name: Enter your name
    - Groups: anycompany-salesmarketing
9. Repeat the same steps above to note down the one-time password.
10. You should have 2 users as shown below:
    <img width="1507" height="271" alt="image" src="https://github.com/user-attachments/assets/f2ba7634-2ece-48cc-9f18-97b0c86f8693" />

### Sign up for QuickSight
1. Navigate to QuickSight console and sign up for QuickSight
   <img width="1212" height="180" alt="image" src="https://github.com/user-attachments/assets/b2594a90-d7f2-42be-b6be-a3b936598097" />
   <img width="995" height="414" alt="image" src="https://github.com/user-attachments/assets/716ca1de-5628-4044-9956-a77454b1651f" />

2. Enter the following details:
   - Email: Enter your email
   - Authentication Method: Use AWS IAM Identity Center
   - QuickSight Region: Asia Pacific (Singapore)
   - QuickSight account name: eg. quicksight-account-03092025
   - <img width="1505" height="733" alt="image" src="https://github.com/user-attachments/assets/f5024f71-70a7-44f6-bb76-f004e84cc51c" />
   - Click Configure
   - Click Show more roles
   - Add anycompany-admin to Admin Pro Group
   - Add anycompany-salesmarketing to Reader Pro Group
   - <img width="970" height="494" alt="image" src="https://github.com/user-attachments/assets/ab2069d2-6204-421d-8fb8-f8be9cea4f08" />
3. QuickSight access to AWS services: Leave as default.
4. Uncheck "Add Pixel-Perfect Reports"
5. Click Finish
6. Once the QuickSight account creation is completed, click "Go to QuickSight"
   <img width="1506" height="580" alt="image" src="https://github.com/user-attachments/assets/edd887b7-0294-4414-af67-81912c621a65" />
   
7. You should see the homepage of QuickSight as below:
   <img width="1505" height="704" alt="image" src="https://github.com/user-attachments/assets/fa37ec57-878a-4419-b55e-ca47fde5446f" />

### Create a VPC (Optional)
We'll create a dedicated SageMaker VPC with 3 public + 3 private subnets. We'll deploy SageMaker Unified Studio into this VPC later.
1. If you are using a workshop account, you can skip this step.
2. Download sagemaker-infrastructure.yaml file from this Github repo.
3. Search for CloudFormation > Create Stack
   - Choose an existing template
   - Specify Template: Upload a template file. Upload the downloaded template file.
   - Click Next
   - Stack Name: eg. sagemaker-vpc
   - Click Next
   - Leave as defaults
   - Click Next > Click Submit
   - <img width="1510" height="493" alt="image" src="https://github.com/user-attachments/assets/41902dee-4644-4ff0-a0b4-1e98414592ca" />

### Create Amazon SageMaker Studio Domain
1. Navigate to the Amazon SageMaker console
3. Select "Create a Unified Studio domain"
   - <img width="1061" height="421" alt="image" src="https://github.com/user-attachments/assets/02af4a34-699e-4705-8647-a0cc8d475e17" />
   - Quick setup
   - Expand Quick setup settings
   - Domain Name: eg. sagemaker-workshop-domain
   - VPC: sagemaker-workshop
   - Subnet: Select all 3 private subnets
   - Leave the others as default
   - Click continue
   - <img width="1481" height="684" alt="image" src="https://github.com/user-attachments/assets/530ac43b-b5e1-44f0-acf4-4e891fd2637a" />
   - Create IAM Identity Center User: dg-corp-admin
   - <img width="1494" height="458" alt="image" src="https://github.com/user-attachments/assets/9f81f788-333c-4fd7-9e9f-6800dbf258cd" />
   - Click Create Domain
14. Once the domain is created, click on User Management > Users
   - Click Actions > Add SSO users and groups
   - Add dg-business-analyst user
   - <img width="1495" height="442" alt="image" src="https://github.com/user-attachments/assets/9fb643fb-7db5-4dd4-ba2a-2bf3dd7f935c" />
   - <img width="1493" height="468" alt="image" src="https://github.com/user-attachments/assets/71808295-5d14-4b5f-8bd4-6fbbd52a0f8d" />
   
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
