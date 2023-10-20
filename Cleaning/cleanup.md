# Simulatenously delete resources from multiple cloud providers
First step - Empty your bucket
- Access the AWS Console and open S3 Service.
- Select your bucket and click on Empty.
- To confirm deletion, type permanently delete in the text input field.
- Click on Empty

Next: Set the instance deletion_protection from true to false
Open the Google Editor, and access the following path: mission1_en/mission1/en/terraform/. Now, open the tcb_gcp_database.tf file.
On line 11 of the tcb_gcp_database.tf file:
Replace from deletion_protection = “true” to deletion_protection = “false”.
Change from Editor to Terminal.

Run the commands to apply the terraform file changes:
```sh
cd ~/mission1_en/mission1/en/terraform/

terraform init
terraform apply
	Enter a value: yes
```

Delete all the resources with the terraform destroy.
```sh
terraform destroy
	Enter a value: yes
```

Finished.
