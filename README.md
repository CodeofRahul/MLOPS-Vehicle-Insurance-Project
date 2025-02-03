# MLOPS-Vehicle-Project


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

