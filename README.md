# NFL Data Lake - AWS

## Project Guidelines by Alicia Ahl:
[Watch the Tutorial](https://www.youtube.com/watch?v=RAkMac2QgjM&t=1040s)

### Sample Data from Athena Query
![NFLDataLake](athenaquery_sampleoutput.csv)

This repository contains the setup_nfl_data_lake.py script, which automates the creation of a data lake for NFL analytics using AWS services. The script integrates Amazon S3, AWS Glue, and Amazon Athena, and sets up the infrastructure needed to store and query NFL-related data.

---

## Project Overview

The setup_nfl_data_lake.py script performs the following actions:

- Creates an Amazon S3 bucket to store raw and processed data.
- Uploads sample NFL player data (JSON format) to the S3 bucket.
- Creates an AWS Glue database and an external table for querying the data.
- Configures Amazon Athena for querying data stored in the S3 bucket.
  
## Project Structure

bash
code
```
NFLDataLake/
│
├── src/
│   ├── __init__.py
│   └── setup_nfl_data_lake.py
│
├── tests/  # Placeholder for test files
│
├── data/  # Placeholder for raw or processed data
│
├── requirements.txt
├── README.md
└── .env  # Environment variables for secure configurations
```

## Prerequisites

### 1. AWS Account Setup

### Create an AWS user with the following permissions:

- AmazonS3FullAccess
- AWSGlueServiceRole
- AmazonAthenaFullAccess

### Generate and securely store AWS access keys.

### 2. Environment Variables
   
Add the following keys in the .env file:

env
code
```
AWS_ACCESS_KEY_ID=your_access_key_id
AWS_SECRET_ACCESS_KEY=your_secret_access_key
SPORTS_DATA_API_KEY=your_sportsdata_api_key
NFL_ENDPOINT=https://api.sportsdata.io/v3/nfl/scores/json/Players
```

### 3. Install Required Packages
Run the following command to install necessary dependencies:

bash
code

```
pip install -r requirements.txt
```
### Ensure the following packages are included in requirements.txt:

- boto3
- requests
- python-dotenv
  
## Setup Instructions

### Step 1: Configure AWS Resources

Run the Python script:

bash
code

```
python3 src/setup_nfl_data_lake.py
```
#### The script will:

- Create the necessary S3 bucket.
- Fetch and upload NFL player data.
- Set up a Glue database and table.
- Configure Athena for querying the data.
  
### Step 2: Verify Resources

**S3 Bucket**: Ensure the bucket contains raw-data/nfl_player_data.jsonl.

**Glue Table**: Verify the nfl_players table in the AWS Glue Console.

**Athena Query**: Run a sample query to verify data is accessible:

sql
code
```
SELECT FirstName, LastName, Position, Team
FROM nfl_players
WHERE Position = 'QB';
```

## Common Issues

### 1. Invalid AWS Access Key Error

Issue: 
- When running the script, you encounter an error like:

vbnet
code

```
An error occurred (InvalidAccessKeyId) when calling the CreateBucket operation: The AWS Access Key Id you provided does not exist in our records.
```
Cause:

- Incorrect or missing AWS access key or secret access key in the .env file.
- The access key might be expired or revoked.

Solution:

- Double-check that your .env file contains the correct AWS credentials.
- Ensure the IAM user associated with the keys has appropriate permissions (AmazonS3FullAccess, AWSGlueServiceRole, AmazonAthenaFullAccess).
- If using AWS CloudShell or EC2, ensure the appropriate IAM role is attached with the required permissions.
  
### 2. Invalid or Missing NFL Endpoint

Issue:
- Error when fetching NFL data:

mathematica
code
```
Invalid URL 'None': No scheme supplied. Perhaps you meant https://None?
```
Cause:

- The NFL_ENDPOINT environment variable is not set or incorrectly configured in the .env file.
- The endpoint might be outdated or incorrect.

Solution:

- Verify the NFL_ENDPOINT variable is correctly set in the .env file:

env
code
```
NFL_ENDPOINT=https://api.sportsdata.io/v3/nfl/scores/json/Players
```
- Check if your API subscription is active and the endpoint is accessible.
- Ensure your API key is valid and not expired.

### 3. Athena Query Result Location Not Configured

Issue:
- Athena query fails with an error like:

graphql
code
```
Query failed: No query result location set for the query.
```
Cause:

- The S3 bucket for Athena query results was not properly configured.
  
Solution:

- Set the Athena query result location to your S3 bucket:

  - Navigate to the Athena console.

  - Click "Settings."

  - Set the query result location to the bucket created by your script:

arduino
code
```
s3://your-bucket-name/athena-results/
```
- Ensure the bucket has the correct permissions for Athena to store query results.

## Key Learnings

- AWS Services Integration: Efficient use of AWS S3, Glue, and Athena.
- Data Automation: Automating infrastructure setup and data ingestion.
- Secure Configuration: Managing access keys and API tokens securely.
