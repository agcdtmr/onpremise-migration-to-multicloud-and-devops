# On-premise migration to Multicloud and DevOps Project


Overall hours Spent for this project: 

- [x] [Part 1](https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/README.md#part-1-enable-amulticloud-architecturedeployment-throughterraform-with-resources-running-inawsandgoogle-cloud-platform): Enable a MultiCloud architecture deployment through Terraform, with resources running in AWS and Google ﻿Cloud Platform
- [x] [Part 2](https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/README.md#part-2-convert-a-database-and-an-application-to-run-on-themulticloud-architectureawsandgoogle-cloud-includingdockerandkubernetes): Convert a database and an application to run on the MultiCloud architecture (AWS ﻿and ﻿Google Cloud), including Docker and Kubernetes
- [x] [Part 3](https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/README.md#part-3-migrate-the-application-files-and-data-from-a-database): Migrate the application files and data from a database
- [x] Delete all the resources created. Destroying the environment permanently. 


## Tech Stack
- Terraform
- AWS S3
- Google ﻿Cloud Platform
- Docker
- Kubernetes


## Part 1: Enable a MultiCloud architecture deployment through Terraform, with resources running in AWS and Google ﻿Cloud Platform

- [x] Create a AWS Free Account
- [x] Create a GCP Free Account
- [x] Got to AWS Console. Create "terraform-en-1" user using the IAM service
- Access the AWS console ([https://aws.amazon.com](https://aws.amazon.com/)) **and log in with your newly created account**. In the search bar, type IAM. In the Services section, click on IAM.
- Click on Users and then Add users, enter the name **terraform-en-1** and click Next to create a programmatic type user.
- Set Permissions: AmazonS3FullAccess
- Review all the details
- Click on **Create user**
- [x] Create the Access Key for the terraform-en-1 user using the IAM service
- Access the **terraform-en-1** user
- At Security credentials tab
- Navigate to the **Access keys** section
- Click on **Create access key**
- Select Command Line Interface (CLI) and I understand the above recommendation and want to proceed to create an access key.
- Click on **Next** and click on **Create access key**
- **Make sure to DOWNLOAD .csv file**
- Once the download is complete, **rename the .csv file to key.csv**
- [x] On Google ﻿Cloud Platform. Create a [Google Cloud project](https://developers.google.com/workspace/guides/create-project). Name it multicloud-project
- [x] Prepare the environment to run Terraform
- Access the Google Cloud Console ([console.cloud.google.com](http://console.cloud.google.com/)) **and log in with your newly created account**
- Open the Cloud Shell
- Download the part1.zip file in the Google Cloud shell using the wget command:
```
wget https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/part1.zip
```
- Upload the key.csv file to the Cloud Shell using the browser
- Verify if the part1.zip and key.csv files are in the folder in the Cloud Shell using the command below
```
ls
```
- Execute the file preparation commands:
```
unzip part1.zip

​
mv key.csv part1/en

​
cd part1/en

​
chmod +x *.sh
```
- Execute the commands below to prepare the AWS and GCP environment
```
mkdir -p ~/.aws/

​
touch ~/.aws/credentials_multiclouddeploy

​
./aws_set_credentials.sh key.csv

​
GOOGLE_CLOUD_PROJECT_ID=$(gcloud config get-value project)

​
gcloud config set project $GOOGLE_CLOUD_PROJECT_ID
```
- Click on Authorize
- Execute the command below to set the project in the Google Cloud Shell
```
./gcp_set_project.sh
```
- Execute the commands to enable the Kubernetes, Container Registry, and Cloud SQL APIs
```
gcloud services enable containerregistry.googleapis.com

​
gcloud services enable container.googleapis.com

​
gcloud services enable sqladmin.googleapis.com

​
gcloud services enable cloudresourcemanager.googleapis.com

​
gcloud services enable serviceusage.googleapis.com
​
gcloud services enable compute.googleapis.com
​
gcloud services enable servicenetworking.googleapis.com --project=$GOOGLE_CLOUD_PROJECT_ID
```
- [x] Run Terraform to provision MultiCloud infrastructure in AWS and Google Cloud
- [x] Execute the following commands to provision infrastructure resources
```
cd ~/part1/en/terraform/

​
terraform init

​
terraform plan

​
terraform apply
```

Attention: The provisioning process can take between 15 to 25 minutes to finish. Keep the CloudShell open during the process. If disconnected, click on Reconnect when the session expires (the session expires after 5 minutes of inactivity by default)

### Security Tips

- For production environments, it's recommended to use only the Private Network for database access.
- Never provide public network access (0.0.0.0/0) to production databases. ⚠️

- [ ] Go to GCP Cloud SQL
- [ ] Go to GCP Kubernetes Engine
- [ ] Go to AWS Console Amazon S3


### Part 1: Destroying the environment and starting over

In case you have encountered any problem/error and want to reset the environment to start over, follow the step-by-step instructions below to remove the entire MultiCloud environment:

- [ ] [Google Cloud] Delete VPC Peering
- [ ] [Google Cloud] Delete remaining resources w/ Terraform - Cloud Shell
```
cd ~/part1/en/terraform/
​
terraform destroy
```
- [ ] Clean the Cloud Shell in AWS
```
cd ~
​
rm -rf mission*
```
- [ ] Clean the Cloud Shell in Google Cloud
```
cd ~
​
rm -rf mission*
​
rm -rf .ssh
```


## Part 2: Convert a database and an application to run on the MultiCloud architecture (AWS ﻿and ﻿Google Cloud), including Docker and Kubernetes

- [x] Access AWS console and go to IAM service. Create "luxxy-covid-testing-system-en-app1" user using the IAM service
- Under Access management, Click in "Users", then "Add users". Insert the User name **luxxy-covid-testing-system-en-app1** and click in **Next** to create a programmatic user.
- Set Permissions: AmazonS3FullAccess
- Review all the details
- Click on **Create user**
- [x] Create access key
- Select **Command Line Interface (CLI)** and **I understand the above recommendation and want to proceed to create an access key** checkbox.
- Click Next
- Click on Create access key
- Click on Download .csv file
- After download, click Done.
- Now, rename .csv file downloaded to **luxxy-covid-testing-system-en-app1.csv**
- [x] In Google Cloud Platform (GCP). Navigate to Cloud SQL instance and create a new user "app" with password welcome123456 on Cloud SQL MySQL database
- [x] Connect to Google Cloud Shell
- [x] **Download** the part2 files to Google Cloud Shell using the wget command as shown below
```
cd ~
wget https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/part2.zip
unzip part2.zip
```
- [x] Connect to MySQL DB running on Cloud SQL (once it prompts for the password, provide welcome123456). Don’t forget to replace the placeholder with your Cloud SQL Public IP
```
mysql --host=<replace_with_public_ip_cloudsql> --port=3306 -u app -p
```
- [x] Once you're connected to the database instance, create the products table for testing purposes
```
use dbcovidtesting;
​
source ~/part2/en/db/create_table.sql
​
show tables;
​
exit;
```
- [x] Enable Cloud Build API via Cloud Shell.
```
gcloud services enable cloudbuild.googleapis.com
```
- [x] Build the Docker image and push it to Google Container Registry.
```
GOOGLE_CLOUD_PROJECT_ID=$(gcloud config get-value project)
```
    
```
cd ~/part2/en/app
```
    
```
gcloud builds submit --tag gcr.io/$GOOGLE_CLOUD_PROJECT_ID/luxxy-covid-testing-system-app-en
```
- [x] Open the Cloud Editor, go to part2 directory and edit the Kubernetes deployment file (luxxy-covid-testing-system.yaml) and update the variables below with your <PROJECT_ID> on the Google Container Registry path, AWS Bucket name, AWS Keys (open file luxxy-covid-testing-system-en-app1.csv and use Access key ID and Secret access key)  and Cloud SQL Database Private IP.
```
cd ~/part2/en/kubernetes
```

```
Sample of luxxy-covid-testing-system.yaml file:

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
- [x] Connect to the GKE (Google Kubernetes Engine) cluster via Console. Go to GKE (Google Kubernetes Engine) **Clusters**. Find "luxxy-kubernetes-cluster-en" and click *Connect*. And Click "Run in Cloud Shell"
- [x] Deploy the application Luxxy in the Cluster
    
    ```
    cd ~/part2/en/kubernetes
    ```
    
    ```
    kubectl apply -f luxxy-covid-testing-system.yaml
    ```
    
- [x] Under **GKE** > **Workloads** > **Exposing Services**, get the application Public IP. You should see the app up & running!

![App Screenshot](https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/Screenshot%202024-02-06%20at%2009.48.48.png)

- [x] (Optional) Download [PDF](https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/covid-testing.pdf) and add an entry in the application for testing in "Add Guest Results" button

![test entry screenshot](https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/Screenshot%202024-02-06%20at%2009.49.14.png)

### Part 2: # Destroying the environment and starting over

In case you have encountered any problem/error and want to reset the environment to start over, follow the step-by-step instructions below to remove the entire MultiCloud environment.

- [ ] [Google Cloud] Delete Kubernetes resources. Go to GKE (Google Kubernetes Engine) **Clusters**. Find "luxxy-kubernetes-cluster-en" and click *Connect*. And Click "Run in Cloud Shell". And use the command below to delete Kubernetes resources
```
kubectl delete deployment luxxy-covid-testing-system

kubectl delete service luxxy-covid-testing-system

```
- [ ] [Google Cloud] Delete VPC Peering
- [ ] [AWS] Delete file inside of S3
- [ ] [Google Cloud] Delete remaining resources w/ Terraform - Cloud Shell
```
cd ~/part1/en/terraform/

terraform destroy
```
- [ ] Clean the Cloud Shell in AWS
```
cd ~
​
rm -rf mission*
```
- [ ] Clean the Cloud Shell in Google Cloud
```
cd ~
​
rm -rf mission*
​
rm -rf .ssh
```


## Part 3: Migrate the application files and data from a database

- [x] Connect to Google Cloud Shell
- [x] Download the test dumps using wget
```
cd ~
​
wget https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/part3.zip
​
unzip part3.zip
```
- [x] Connect to MySQL DB running on Cloud SQL (once it prompts for the password, provide welcome123456). Don’t forget to replace the placeholder with your Cloud SQL Public IP
```
mysql --host=<replace_with_public_ip_cloudsql> --port=3306 -u app -p
```
- [x] Import the test dump on Cloud SQL
```
use dbcovidtesting;
​
source ~/part3/en/db/db_dump.sql
```
- [x] Check if the data got imported correctly
```
select * from records;
​
exit;
```
- [x] Connect to the AWS Cloud Shell
- [x] Download the PDF files
```
wget https://github.com/agcdtmr/onpremise-migration-to-multicloud-and-devops/blob/main/part3.zip

unzip mission3.zip
```
- [x] Sync PDF Files with your AWS S3 used for COVID-19 Testing Status System. Replace the bucket name with yours.
```
cd mission3/en/pdf_files
```

```
aws s3 sync . s3://**luxxy-covid-testing-system-pdf-en-xxxx**
```
- [x] Test the application. Upon migrating the data and files, you should be able to see the entries  under “View Guest Results” page. Successfully migrated an "on-premises" application & database to a MultiCloud Architecture!

## Delete all the resources created. Destroying the environment permanently.

After completing the this project and gathering the implementation evidence, follow the step-by-step instructions below to remove the entire MultiCloud environment.

- [ ] [Google Cloud] Delete Kubernetes resources. Go to GKE (Google Kubernetes Engine) **Clusters**. Find "luxxy-kubernetes-cluster-en" and click *Connect*. And Click "Run in Cloud Shell". And use the command below to delete Kubernetes resources
```
kubectl delete deployment luxxy-covid-testing-system

kubectl delete service luxxy-covid-testing-system

```

- [ ] [Google Cloud] Delete VPC Peering
- [ ] [AWS] Delete files inside of S3
- [ ] [Google Cloud] Delete remaining resources w/ Terraform - Cloud Shell
```
cd ~/mission1/en/terraform/
​
terraform destroy
```
- [ ] Clean the Cloud Shell in AWS
```
cd ~
​
rm -rf mission*
```
- [ ] Clean the Cloud Shell in Google Cloud
```
cd ~
​
rm -rf mission*
​
rm -rf .ssh
```
