# MLOps Project - Vehicle Insurance Data Pipeline

Welcome to this MLOps project, designed to demonstrate a robust pipeline for managing vehicle insurance data. This project aims to showcase the various tools, techniques, services, and features that go into building and deploying a machine learning pipeline for real-world data management.
---

## 📁 Project Setup and Structure

### Step 1: Project Template

- Start by executing the template.py file to create the initial project template, which includes the required folder structure and placeholder files.

### Step 2: Package Management

- Write the setup for importing local packages in setup.py and pyproject.toml files.
- Tip: Learn more about these files from crashcourse.txt.

### Step 3: Virtual Environment and Dependencies

- Create a virtual environment and install required dependencies from `requirements.txt`:

```
conda create -p vehicle python=3.10 -y
conda activate vehicle
pip install -r requirements.txt
```

- Verify the local packages by running:

```
pip list
```

---

## 📊 MongoDB Setup and Data Management

### Step 4: MongoDB Atlas Configuration

1. Sign up for MongoDB Atlas and create a new project.
2. Set up a free M0 cluster, configure the username and password, and allow access from any IP address (`0.0.0.0/0`).
3. Retrieve the MongoDB connection string for Python and save it (replace `<password>` with your password).

### Step 5: Pushing Data to MongoDB

1. Create a folder named `notebook`, add the dataset, and create a notebook file `mongoDB_demo.ipynb`.
2. Use the notebook to push data to the MongoDB database.
3. Verify the data in MongoDB Atlas under Database > Browse Collections.

---

## 📝 Logging, Exception Handling, and EDA

### Step 6: Set Up Logging and Exception Handling

- Create logging and exception handling modules. Test them on a demo file `demo.py`.

### Step 7: Exploratory Data Analysis (EDA) and Feature Engineering

- Analyze and engineer features in the `EDA` and `Feature Engg` notebook for further processing in the pipeline.

---

## 📥 Data Ingestion

### Step 8: Data Ingestion Pipeline

- Define MongoDB connection functions in `configuration.mongo_db_connections.py`.
- Develop data ingestion components in the `data_access` and `components.data_ingestion.py` files to fetch and transform data.
- Update `entity/config_entity.py` and `entity/artifact_entity.py` with relevant ingestion configurations.
- Run `demo.py` after setting up MongoDB connection as an environment variable.

### Setting Environment Variables

- **Set MongoDB URL:**

```
# For Bash
export MONGODB_URL="mongodb+srv://<username>:<password>...."
# For Powershell
$env:MONGODB_URL = "mongodb+srv://<username>:<password>...."
```

- **Note:** On Windows, you can also set environment variables through the system settings.

---

## 🔍 Data Validation, Transformation & Model Training

### Step 9: Data Validation

- Define schema in `config.schema.yaml` and implement data validation functions in `utils.main_utils.py`.

### Step 10: Data Transformation

- Implement data transformation logic in `components.data_transformation.py` and create `estimator.py` in the `entity` folder.

### Step 11: Model Training

- Define and implement model training steps in `components.model_trainer.py` using code from `estimator.py`.

---

## 🌐 AWS Setup for Model Evaluation & Deployment

### Step 12: AWS Setup

1. Log in to the AWS console, create an IAM user, and grant `AdministratorAccess`.

2. Set AWS credentials as environment variables

```
# For Bash
export AWS_ACCESS_KEY_ID="YOUR_AWS_ACCESS_KEY_ID"
export AWS_SECRET_ACCESS_KEY="YOUR_AWS_SECRET_ACCESS_KEY"
```

3. Configure S3 Bucket and add access keys in `constants.__init__.py`.

### Step 13: Model Evaluation and Pushing to S3

- Create an S3 bucket named `my-model-mlopsproj11` in the `us-east-1` region.
- Develop code to push/pull models to/from the S3 bucket in `src.aws_storage` and `entity/s3_estimator.py`.

---

## 🚀 Model Evaluation, Model Pusher, and Prediction Pipeline

### Step 14: Model Evaluation & Model Pusher

- Implement model evaluation and deployment components.
- Create `Prediction Pipeline` and set up `app.py` for API integration.

### Step 15: Static and Template Directory

- Add `static` and `template` directories for web UI.

---

## 🔄 CI/CD Setup with Docker, GitHub Actions, and AWS

### Step 16: Docker and GitHub Actions

1. Create `Dockerfile` and `.dockerignore`.

2. Set up GitHub Actions with AWS authentication by creating secrets in GitHub for:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_DEFAULT_REGION`
- `ECR_REPO`

### Step 17: AWS EC2 and ECR

1. Set up an EC2 instance for deployment.
2. Install Docker on the EC2 machine.
3. Connect EC2 as a self-hosted runner on GitHub.

### Step 18: Final Steps

- Open the 5080 port on the EC2 instance.
- Access the deployed app by visiting `http://<public_ip>:5080`.

---

## 🛠️ Additional Resources

- **Crash Course on setup.py and pyproject.toml:** See `crashcourse.txt` for details.
- **GitHub Secrets:** Manage secrets for secure CI/CD pipelines.

---

## 🎯 Project Workflow Summary

1. **Data Ingestion ➔ Data Validation ➔ Data Transformation**
2. **Model Training ➔ Model Evaluation ➔ Model Deployment**
3. **CI/CD Automation** with GitHub Actions, Docker, AWS EC2, and ECR

---






- Install AWS CLI : https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

## **Problem**
I ran s3_resource.meta.client.upload_file(PATH_IN_COMPUTER, BUCKET_NAME, KEY) The code ran without errors but the file did not get uploaded.

## **Solution**

### 🔍 Step 1: Check If AWS CLI Recognizes the Credentials

Run the following command:

```cmd
aws sts get-caller-identity
```

If credentials are correct, you should see output like:

```JSON
{
    "UserId": "ABC123XYZ456",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/your-user"
}
```

- ✅ If this works → Your credentials are fine; move to Step 4.
- ❌ If you get an error (e.g., "InvalidAccessKeyId") → Move to Step 2.

### 🔄 Step 2: Unset Environment Variables & Use AWS CLI

Unset the incorrectly set environment variables:

```CMD
set AWS_ACCESS_KEY_ID=
set AWS_SECRET_ACCESS_KEY=
```

Then, configure AWS CLI properly using:

```cmd
aws configure
```

### 🚀 Step 3: Test S3 Access Again

Run:

```cmd
aws s3 ls
```

- ✅ If this lists S3 buckets → Your credentials work. Try running your Python script again.
- ❌ If the error persists → Double-check your access keys in the AWS Console (IAM → Users → Security Credentials).

While running `aws sts get-caller-identity` if you're getting `'aws' is not recognized as an internal or external command,
operable program or batch file.`

### 🔍 Step 1: Check If AWS CLI Is Installed

Run this command to check if AWS CLI is installed:

```cmd
where aws
```

- ✅ If it outputs a path like `C:\Program Files\Amazon\AWSCLI\bin\aws.exe` → Move to Step 3.
- ❌ If it says `INFO: Could not find files for the given pattern(s).` → Move to Step 2.

### 🔄 Step 2: Install AWS CLI

If AWS CLI is not installed, download and install it:

**1. Download AWS CLI**

- Go to: [AWS CLI Installe](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

- Run the installer (`AWSCLIV2.msi`) and follow the setup steps

**Verify Installation**

After installation, close and reopen CMD, then check:

```cmd
aws --version
```

- ✅ If it shows something like `aws-cli/2.x.x` → Installation is successful.

Then, try:

```cmd
aws sts get-caller-identity
```

### 🔄 Step 5: Configure AWS CLI

If the previous step worked, configure AWS credentials:

```cmd
aws configure
```

Enter:

- **AWS Access Key ID**
- **AWS Secret Access Key**
- **Region Name** (`e.g., us-east-1`)
- **Output format** (`json or text`)

Then, test S3 access:

```
aws s3 ls
```

Great! Now that `AWS CLI is working`, let's verify everything step by step before running your Python script again.

### ✅ Step 1: Verify AWS Credentials

Run:

```cmd
aws sts get-caller-identity
```

Expected Output:

```json
{
    "UserId": "ABC123XYZ456",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/your-user"
}
```

- ✅ **If this works** → Your credentials are valid.
- ❌ **If it still says** `InvalidAccessKeyId` → Check IAM permissions & reconfigure AWS (`aws configure`).

### ✅ Step 2: Check S3 Bucket Access

Run:

```cmd
aws s3 ls
```

**Expected Output:** <br>
A list of your S3 buckets, e.g.,

```perl
2025-02-03  my-model-mlopsproj
```

- ✅ **If your bucket appears** → Move to Step 3.
- ❌ **If no bucket appears or access is denied** → You might not have S3 permissions in IAM.

### ✅ Step 3: Try Uploading a File via CLI

Before running your Python script, test uploading a sample file manually:

```cmd
echo "Hello MLOps" > test_file.txt
aws s3 cp test_file.txt s3://my-model-mlopsproj/
```

If this works, check if the file is in S3:

```cmd
aws s3 ls s3://my-model-mlopsproj/
```

- ✅ **If upload works** → AWS setup is fine. Move to Step 4.
- ❌ **If upload fails** → You might not have s3:PutObject permissions.

### ✅ Step 4: Run Your Python Script Again

Now, rerun your Python script:

```cmd
python your_script.py
```

