# Intensive cloud Bootcamp: Part 1

I recently took  part in the intensive cloud training by "The Cloud Bootcamp LLC"
In the project we were tasked with creating a highly available, resilient and secure web application using a multi-cloud strategy ie Google and AWS. 

The first step was to get setup on AWS and GCP with secure accounts.
To get setup on these two providers, utilize their free accounts. 

### Getting started
To get started on AWS, setup your account as per: 
 * [AWS Setup](https://github.com/Stevecmd/AWS-GCP-Multicloud-Bootcamp/blob/master/Mission%201/Steps%20to%20create%20your%20AWS%20Free%20Account.pdf)
 * [Google Setup](https://github.com/Stevecmd/AWS-GCP-Multicloud-Bootcamp/blob/master/Mission%201/Steps%20to%20create%20your%20GCP%20Free%20Account.pdf)

## AWS
Once setup, the next step was to create the user whose permissions will be used to run Terraform on AWS. We created a user named: "terraform-en-1" and assigned them the permissions: "AmazonS3FullAccess."
We then created an access key for the user: "terraform-en-1" so that we can have programmatic access. Once the key is created, make sure to download the file, rename it to "accessKeys.csv" and store it for use later.

## GCP
Firstly, we download the project setup files by the name: "mission1.zip".
Once you access the GCP console, drag and drop the files accessKeys.csv and mission1.zip onto the GCP cloud shell.
Confirm that the files have been uploaded successfully by running the code:"ls-a" in your cloud shell terminal to verify the prescence of the files.

```sh
mkdir mission1_en
mv mission1.zip mission1_en
cd mission1_en
unzip mission1.zip
mv ~/accessKeys.csv mission1/en
cd mission1/en
chmod +x *.sh
```

Run the following commands to prepare the AWS and GCP environments. Authorize when the prompt comes up.

```sh
./aws_set_credentials.sh accessKeys.csv
gcloud config set project <project_id>
./gcp_set_project.sh
```

Next we should enable the Container Registry API, Kubernetes Engine API and the Cloud SQL API

```sh
gcloud services enable containerregistry.googleapis.com 
gcloud services enable container.googleapis.com 
gcloud services enable sqladmin.googleapis.com
```

### NB:
Before executing the Terraform commands, open the Google Editor and update the file tcb_aws_storage.tf replacing the bucket name with an unique name (AWS requires unique bucket names).
- Open the tcb_aws_storage.tf using Google Editor.
- On line 4 of the file tcb_aws_storage.tf:
 - Replace xxxx with your name initials, using 5 letters plus 5 random numbers: Example: luxxy-covid-testing-system-pdf-en-<unique-alphanumeric-combination>
Run the following commands to finish provision infrastructure steps:

```sh
cd ~/mission1_en/mission1/en/terraform/ 
terraform init 
terraform plan 
terraform apply
```

Type Yes and proceed.
Note: The Cloud SQL database may take 15 to 25 minutes to create, keep an eye on the CloudShell and click Reconnect ifthe session expires (the session expires after 5 minutes of inactivity by default).
SQL Network Configuration.
Once the Cloud SQL instance is provisioned, access the Cloud SQL service
Click on your Cloud SQL instance.
On the left side, under Primary Instance, click on Connections.
Go to Networking tab.
Under Instance IP assignment, select Private IP to enable.
Under Associated networking, select "Default"
Click Set up Connection.
Click on Enable API, to enable Service Networking API (if asked).
Select Use an automatically allocated IP range in your network.
Click Continue.
Click Create Connection and wait a minutes until conclude. You will see the message:
"Private services access connection for network default has been successfully created."
Under Authorized Networks, click "Add Network".
Under New Network, enter the following information:
Name: Public Access (For testing purposes only)
Network: 0.0.0.0/0
Click Done.
Click Save and wait to finish the update. This update may take from 10 to 20 minutes to finish.

PS: For production environments, it is recommended to use only the Private Network for database access.

NB: Never grant public network access (0.0.0.0/0) to production databases.
Files: https://tcb-public-events.s3.amazonaws.com/icp/mission1.zip
