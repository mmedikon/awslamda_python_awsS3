The Lambda function is triggered when a PDF is uploaded to an S3 bucket. It downloads the PDF, encrypts it with a password, and uploads the encrypted PDF back to S3.
Architecture

User
  │
  │ Upload PDF
  ▼
Amazon S3
  │
  │ S3 Event
  ▼
AWS Lambda
  │
  ├── Download PDF
  ├── Encrypt PDF
  └── Upload encrypted PDF
  │
  ▼
Amazon S3

**Features**

    Automatically processes PDF files uploaded to S3
    Encrypts PDFs with a password
    Runs serverlessly using AWS Lambda
    Uses Amazon S3 for input and output files
    No server infrastructure required

**Prerequisites**

Before deploying this project, make sure you have:

  
    Python installed locally if the Lambda function is written in Python
    python packages (boto3,pypdf)
    AWS CLI 
    AWS SAM CLI

    
 Consider using AWS Secrets Manager or AWS Systems Manager Parameter Store for PDF password.




**Testing**

You can test the Lambda function by uploading a PDF to the configured S3 bucket.

Check the Lambda logs in Amazon CloudWatch to verify that the function executed successfully.

****Expected flow:****

PDF uploaded
    ↓
S3 event generated
    ↓
Lambda invoked
    ↓
PDF downloaded
    ↓
PDF encrypted
    ↓
Encrypted PDF uploaded

Error Handling

**Deploy through SAM**

sam build

This creates the build artifacts required for deployment.

You can verify the generated build:

ls .projectfolder/

Validate the SAM Template

Run:

sam validate

Deploy

sam deploy --guided

