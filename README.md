# AWS Analytics Workshop
This README guides participants through setting up a unified analytics environment using Amazon SageMaker Studio and visualizing data in Amazon QuickSight.

## Setup Instructions

### 1. IAM Identity Center Configuration

Set up AWS IAM Identity Center (successor to AWS Single Sign-On):

1. Navigate to the IAM Identity Center console
2. Complete the initial setup if not already configured
3. Create or use an existing permission set with the following permissions:
   - `AmazonSageMakerFullAccess`
   - `AmazonQuicksightFullAccess`
   - `IAMFullAccess` (for workshop purposes only - use more restrictive policies in production)
4. Assign the permission set to your user

### 2. Create Amazon SageMaker Studio Domain

1. Navigate to the Amazon SageMaker console
2. Select "Studio" from the left navigation pane
3. Click "Create domain"
4. Follow the setup wizard:
   - Provide domain name
   - Select "AWS IAM Identity Center" as authentication method
   - Configure default execution role
   - Complete network and storage settings
5. Create at least one user profile for yourself

### 3. Enable QuickSight Blueprint in SageMaker Studio

1. Access the SageMaker Studio domain
2. Navigate to the "Admin settings"
3. Under "Apps", find the QuickSight Blueprint
4. Click "Enable" and follow the configuration prompts
5. Return to your user profile and add the QuickSight Blueprint

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
