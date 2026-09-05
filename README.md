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

    An AWS account
    An S3 bucket
    AWS Lambda
    Appropriate IAM permissions
    Python installed locally if the Lambda function is written in Python
    AWS CLI configured, if deploying through the CLI

**Project Structure**

.
├── README.md
├── lambda_function.py
├── requirements.txt
└── ...
 Consider using AWS Secrets Manager or AWS Systems Manager Parameter Store for PDF password.

S3 Configuration

Configure an S3 event notification to trigger the Lambda function when a PDF is uploaded.

Example:

Bucket: my-pdf-bucket

Trigger:
Object Created

Filter:
Suffix = .pdf

The Lambda function should have permission to:

s3:GetObject
s3:PutObject

for the relevant S3 bucket/prefix.
IAM Permissions

The Lambda execution role requires access to read the uploaded PDF and write the encrypted PDF.

Example policy:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::YOUR-BUCKET/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::YOUR-BUCKET/*"
    }
  ]
}

Replace YOUR-BUCKET with your actual S3 bucket name.
Deployment
1. Install dependencies

If dependencies are required:

pip install -r requirements.txt -t package/

Copy the Lambda source code into the package directory:

cp lambda_function.py package/

Create the deployment ZIP:

cd package
zip -r ../lambda.zip .

2. Create the Lambda function

Upload lambda.zip to AWS Lambda and configure:

Runtime: Python 3.x
Handler: lambda_function.lambda_handler

Adjust the runtime and handler according to your implementation.
Lambda Handler

The Lambda function should follow the standard AWS Lambda handler format:

def lambda_handler(event, context):
    # Read S3 event
    # Download PDF
    # Encrypt PDF
    # Upload encrypted PDF
    pass

Input and Output
Input

An unencrypted PDF is uploaded to the configured S3 bucket.

Example:

input/
└── document.pdf

Output

The Lambda function creates an encrypted version:

output/
└── document_encrypted.pdf

**Testing**

You can test the Lambda function by uploading a PDF to the configured S3 bucket.

Check the Lambda logs in Amazon CloudWatch to verify that the function executed successfully.

Expected flow:

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

The function should handle:

    Invalid S3 events
    Missing PDF files
    Non-PDF uploads
    Encryption failures
    S3 download/upload errors

Errors should be logged to Amazon CloudWatch for troubleshooting.
Security Considerations

    Do not commit passwords, AWS access keys, or other secrets to Git.
    Use AWS IAM roles instead of hard-coded AWS credentials.
    Store sensitive encryption passwords in AWS Secrets Manager or Parameter Store.
    Restrict S3 permissions to only the required bucket and prefixes.
    Enable S3 encryption at rest where appropriate.
    Consider enabling S3 versioning and logging for production workloads.

Troubleshooting
Lambda does not trigger

Check:

    S3 event notification configuration
    Lambda resource permissions
    S3 object-created event configuration
    Prefix/suffix filters

Access Denied

Verify that the Lambda execution role has:

s3:GetObject
s3:PutObject

permissions for the required S3 objects.
Lambda timeout

For large PDFs, consider increasing:

    Lambda timeout


Build the SAM Application

From the project root:

sam build

This creates the build artifacts required for deployment.

You can verify the generated build:

ls .aws-sam/build/

Validate the SAM Template

Run:

sam validate

For additional validation:

sam validate --lint

Deploy

For the first deployment, use:
    
    Lambda memory
    Ephemeral storage
