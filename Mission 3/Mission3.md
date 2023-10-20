

# Google Cloud Platform - Database Migration steps
Connect to Google Cloud Shell
Download the dump using wget

```sh
cd ~
mkdir mission3_en
cd mission3_en
wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
unzip mission3.zip
```
Connect to Cloud SQL MySQL database instance
```sh
mysql --host=<public_ip_address> --port=3306 -u app -p
```

Import the dump on Cloud SQL
```sh
use dbcovidtesting;
source ~/mission3_en/mission3/en/db/db_dump.sql
```

Check if the data got imported correctly
```sh
select * from records;
exit;
```

# Amazon Web Services - PDF Files Migration steps
Connect to the AWS Cloud Shell
Download the PDF files

```sh
mkdir mission3_en
cd mission3_en
wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
unzip mission3.zip
```
Sync PDF Files with your AWS S3 used for COVID-19 Testing Status System. Replace the bucket name with yours.
```sh
cd mission3/en/pdf_files
aws s3 sync . s3://luxxy-covid-testing-system-pdf-en-xxxx
```

Test the application. Upon migrating the data and files, you should be able to see the entries under “View Guest Results” page.
**final records image
