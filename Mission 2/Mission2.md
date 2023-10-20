# Intensive cloud Bootcamp: Part 2

## Amazon Web Services
Access AWS console and go to IAM service
Under Access management, Click in "Users", then "Add users". Insert the User name luxxy-covid-testing-system-en-app1 and click in Next to create a programmatic user.

On Set permissions, Permissions options, click in "Attach policies directly" button.
Type AmazonS3FullAccess in Search.
Select AmazonS3FullAccess

Steps to create access key:
Click on the user you have created.
Go to Security credentials tab.
Scroll down and go to Access keys section.
Click on Create access key

Select Command Line Interface (CLI) and I understand the above recommendation and want to proceed to create an access key checkbox.
Create the access key
Click on Download .csv file
Now, rename .csv file downloaded to luxxy-covid-testing-system-en-app1.csv

## Google Cloud Platform (GCP)
Navigate to Cloud SQL instance and create a new user app with password welcome123456 on Cloud SQL MySQL database.
Connect to Google Cloud Shell
Download the mission2 files to Google Cloud Shell using the wget command as shown below

```sh
cd ~
mkdir mission2_en
cd mission2_en
wget https://tcb-public-events.s3.amazonaws.com/icp/mission2.zip
unzip mission2.zip
```

Connect to MySQL DB running on Cloud SQL (once it prompts for the password, provide welcome123456)

```sh
mysql --host=<public_ip_cloudsql> --port=3306 -u app -p
```

Once you're connected to the database instance, create the products table for testing purposes
```sh
use dbcovidtesting;
source ~/mission2_en/mission2/en/db/create_table.sql
show tables;
exit;
```

Enable Cloud Build API via Cloud Shell.
```sh
# Command to enable Cloud Build API

gcloud services enable cloudbuild.googleapis.com
```

Known issue during this step
If you see the error below, please follow the steps to fix it:
```sh
ERROR: (gcloud.builds.submit) INVALID_ARGUMENT: could not resolve source: googleapi: Error 403: 989404026119@cloudbuild.gserviceaccount.com does not have storage.objects.get access to the Google Cloud Storage object., forbidden

To solve it:

1. Access IAM & Admin;
2. Click on check-box Include Google-provided role grants;
3. Click and select your Cloud Build Service Account

Example: 989404026119@cloudbuild.gserviceaccount.com Cloud Build Service Account

4. On your Cloud Build Service Account, right side, click on Edit principal
5. Click on Add another role
6. Click on Select Role, and filter by Storage Admin or gcs. Select Storage Admin (Full control of GCS resources).
7. Click on Save and go to Cloud Shell.
```
Build the Docker image and push it to Google Container Registry. Please replace the <PROJECT_ID> with your My First Project ID. 

```sh
cd ~/mission2_en/mission2/en/app
gcloud builds submit --tag gcr.io/<PROJECT_ID>/luxxy-covid-testing-system-app-en
```
Open the Cloud Editor and edit the Kubernetes deployment file (luxxy-covid-testing-system.yaml) and update the variables below on line 33 in red with your <PROJECT_ID> on the Google Container Registry path, on line 42 AWS Bucket name, AWS Keys (open file luxxy-covid-testing-system-en-app1.csv and use Access key ID on line 44 and Secret access key on line 46)  and Cloud SQL Database Private IP on line 48.

```sh
cd ~/mission2/en/kubernetes
luxxy-covid-testing-system.yaml

				image: gcr.io/<PROJECT_ID>/luxxy-covid-testing-system-app-en:latest
...
				- name: AWS_BUCKET
          value: "luxxy-covid-testing-system-pdf-en-xxxx"
        - name: S3_ACCESS_KEY
          value: "xxxxxxxxxxxxxxxxxxxxx"
        - name: S3_SECRET_ACCESS_KEY
          value: "xxxxxxxxxxxxxxxxxxxx"
        - name: DB_HOST_NAME
          value: "172.21.0.3"
```
Connect to the GKE (Google Kubernetes Engine) cluster via Console
Deploy the application Luxxy in the Cluster

```sh
cd ~/mission2_en/mission2/en/kubernetes
kubectl apply -f luxxy-covid-testing-system.yaml
```

Get the Public IP and test the application (CLICK HERE to download COVID-19 Testing result sample). Search for GKE, click on Services & Ingress, and then on Endpoints address:port

You should see the app up & running!
