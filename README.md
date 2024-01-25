# The Cloud Resume Challenge

This is my personal website, built using GCP and AWS services as part of the [Cloud Resume Challenge](https://cloudresumechallenge.dev/docs/the-challenge/aws/), accessible at [deepansh.app](https://deepansh.app/).

<!-- ![image](./resume-diagram.png) -->

### [Cloud Resume Frontend](https://github.com/deepansharya1111/cloud-resume-frontend/tree/main)

This repository contains the frontend code for the Cloud Resume Challenge. The website is hosted on Google Cloud Storage (GCS) and Amazon Simple Storage Service (S3), pointing to a single domain: https://deepansh.app.

#### Click here for 👉 [Cloud Resume Backend](https://github.com/deepansharya1111/cloud-resume-backend/tree/main)

## Repository Structure

cloud-resume-frontend/<br>
‎ ├── .github/workflows/<br>
‎ ‎ │‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── aws-workflow.yaml<br>
‎‎‎ ‎ │‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── gcp-workflow.yml<br>
‎ ├── frontend/<br>
‎ ‎ ‎│‎‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── index.html<br>
‎ ‎ │‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── 404.html<br>
‎ ‎ │‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── Deepansh-Singh-Resume.pdf<br>
‎ ‎ │‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── images/<br>
‎ ‎ │‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── css/<br>
‎ ‎ │‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── js/<br>
‎ ‎ │‎ ‎ ‎ ‎ ‎ ‎ ‎ ‎ ├── nsfw/<br>
‎ ‎├── README .md<br>
└── ...<br>

- **.github/workflows/**: Contains GitHub Actions workflows for deploying to GCS and S3.
  - `aws-workflow.yaml`: GitHub Actions workflow for deploying to AWS S3.
  - `gcp-workflow.yml`: GitHub Actions workflow for deploying to Google Cloud Storage.

- **frontend/**: Contains the actual frontend code.
  - `index.html`: The main HTML file for your website.
  - `404.html`: The HTML file displayed when a 404 error occurs. Try entering any path after the main domain :)
  - `images/`: Directory for storing images.
  - `css/`: Directory for storing CSS stylesheets.
  - `js/`: Directory for storing JavaScript files.
  - `nsfw/`: Additional directory for NSFW (Not Safe For Work) content. Accessible at https://deepansh.app/nsfw

## Getting Started

1. Fork or Clone this repository.

2. Navigate to the cloud-resume-frontend directory:
   ```
   cd cloud-resume-frontend
   ```

3. Explore the frontend/ directory to view and modify the website code.

## Deploying

### Preparing the infrastructure:
1. Run the terraform files present at [cloud-resume-backend](https://github.com/deepansharya1111/cloud-resume-backend.git) repository. Read the documentation to help get the infrastructure up and running.
Or 
2. Manually create the infrastructure like GCS bucket and make it public using ACLs. Similarly, manually create the AWS s3 bucket, CloudFront distribution, a lambda function to fetch and update the viewer counter, a DynamoDB table to store the view counter, and Route 53 resources.

### Pushing the website code to GCS bucket with GitHub Actions:

1. Open and edit the gcp-workflow.yml file in .github/workflows/.
2. Modify the values for the Google Cloud Service Account email, key, and bucket name if you plan just to test the code locally:
   ```
   secrets:
     GCP_SA_EMAIL: ${{ secrets.GCP_SA_EMAIL }}
     GCP_SA_KEY: ${{ secrets.GCP_SA_KEY }}
     GCS_BUCKET_NAME: ${{ secrets.GCS_BUCKET_NAME }}
   ```
   Or update these secrets inside your forked GitHub repository settings inside Actions -> secrets -> create new secrets.
3. Push your changes to the main branch, and GitHub Actions will automatically deploy to GCS.
   
## Pushing the website code to AWS S3 bucket with GitHub Actions:

1. Open and edit the aws-workflow.yaml file in .github/workflows/.
2. Modify the values for the AWS S3 bucket, access key, secret access key, and CloudFront distribution:
   ```
   env:
     AWS_S3_BUCKET: ${{ secrets.AWS_S3_BUCKET }}
     AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
     AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
     AWS_CLOUDFRONT_DISTRIBUTION: ${{ secrets.AWS_CLOUDFRONT_DISTRIBUTION }}
   ```
   Or update these secrets inside your forked GitHub repository settings inside Actions -> secrets -> create new secrets.
3. Push your changes to the main branch, and GitHub Actions will automatically deploy to S3 and invalidate the CloudFront cache.


### AWS Deployment Architecture
The Cloud Resume Frontend utilizes the following AWS services:

**S3 Bucket**: The static site is hosted in an S3 bucket.

**Route 53**: Manages DNS and directs requests to CloudFront CDN.

**CloudFront CDN**: Accelerates content delivery, backed by the S3 bucket, and enforces HTTPS.

**AWS Certificate Manager (ACM)**: Issues TLS/SSL certificates for secure HTTPS connections.

**Lambda Function**: A Python Lambda function is triggered by an API Gateway endpoint, updating the visitor count in a DynamoDB database.

**DynamoDB**: Stores the visitor count data.

### GCP Deployment Architecture
The Cloud Resume Frontend utilizes the following GCP services:

**Google Cloud Storage**: The GitHub code gets pushed to a Cloud Storage bucket.

**Application Load Balancer**: Makes the web app code publicly available by exposing the HTTPS endpoint globally.

**Google Cloud CDN**: For caching the web app contents closer to the user.

**SSL Certificate**: Google Certificate Manager is utilized for obtaining an SSL certificate.

**Cloud Function and Firestore**: Cloud Function and Firestore could have been used instead of Lambda and DynamoDB.

**Cloud DNS**: For domain management.
