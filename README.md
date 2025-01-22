# Run Build, Tests, and Deployment on Push

 Steps I followed to set up a CI/CD pipeline using GitHub Actions to build, test, and deploy an AWS Serverless Application Model (SAM) application on a push to the `main` branch. The pipeline runs code linting, installs dependencies, builds the application with SAM, deploys to a test environment, and optionally runs tests after deployment.

## Prerequisites

Before setting up the pipeline, I ensured the following prerequisites were met:

1. **GitHub Account**: I ensured I had a GitHub account with access to the SAM application's repository.
2. **AWS Account**: I ensured I had an AWS account with the necessary permissions to deploy resources using AWS SAM.
3. **AWS CLI**: Installed and configured with appropriate permissions for deploying SAM applications.
4. **AWS SAM CLI**: Installed on the local machine for building and deploying the application.
5. **GitHub Secrets**:
   - `AWS_ACCESS_KEY_ID`: AWS access key for authentication.
   - `AWS_SECRET_ACCESS_KEY`: AWS secret access key.
   - `S3_FOR_SAM`: S3 bucket name for storing SAM deployment artifacts.
6. **Python 3.10**: Ensured that the code is compatible with Python 3.10.
7. **Repository Setup**: I had the SAM application with a `template.yaml`.

## Pipeline Steps

### Step 1: Checkout the Repository
- I configured the pipeline to check out the latest code from the GitHub repository using the `actions/checkout` action.

### Step 2: Set Up Python Environment
- I used the `actions/setup-python` action to set up Python 3.10 on the runner environment to ensure the application ran with the correct Python version.

### Step 3: Install Dependencies (Optional)
- I had the option to install dependencies, such as upgrading `pip` and installing packages from the `requirements.txt` file, by uncommenting the steps for dependency installation.

### Step 4: Run Code Linting
- I added a step to run **code linting** with `flake8` to check the code for style issues. The command targeted critical errors and warnings with the `--select=E9,F63, F7,F82` flag to ensure high code quality.

### Step 5: Configure AWS Credentials for SAM
- I used the `aws-actions/configure-aws-credentials` action to securely configure the AWS credentials in the GitHub environment. The credentials were retrieved from GitHub Secrets (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`), and the `aws-region` was set to `eu-central-1`.

### Step 6: Build the Application with SAM
- I used the `aws-actions/setup-sam` action to set up AWS SAM CLI and then ran the `sam build` command to build the application. This step ensured that the application was correctly prepared for deployment with the `template.yaml` file.

### Step 7: Deploy to the Test Environment
- I deployed the SAM application to a test environment using the `sam deploy` command. The deployment was configured with the following:
  - **No confirmation for changesets** to streamline deployments.
  - **Stack name** set to `aws-resume-stack`.
  - **S3 bucket** retrieved from GitHub Secrets (`S3_FOR_SAM`) to store deployment artifacts.
  - **IAM capabilities** set to `CAPABILITY_IAM` for the necessary permissions.

### Step 8: Run Post-Deployment Tests (Optional)
- After deployment, I ran **optional post-deployment tests** to verify that the Lambda function was deployed correctly. This was done using the `aws lambda invoke` command to trigger the Lambda function and test its response.

## Conclusion
This setup provided a fully automated CI/CD pipeline for building, testing, and deploying my SAM application. The pipeline streamlined the deployment process and ensured that only code that passed linting and tests was deployed to the test environment. I can easily extend this pipeline to include additional steps like notifications or more extensive tests.
## Note
- I ran into an error  with the template.yaml file location.
