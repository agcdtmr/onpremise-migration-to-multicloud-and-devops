# On-premise migration to Multicloud and DevOps Project


Overall hours Spent for this project: 

- [x] Part 1: Enable a MultiCloud architecture deployment through Terraform, with resources running in AWS and Google ﻿Cloud Platform
- [x] Part 2: Convert a database and an application to run on the MultiCloud architecture (AWS ﻿and ﻿Google Cloud), including Docker and Kubernetes
- [x] Part 3: Migrate the application files and data from a database


## Tech Stack
- Terraform
- AWS S3
- Google ﻿Cloud Platform
- Docker
- Kubernetes


## Part 1

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
cd mission1/en

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

## Part 1: Destroying the environment and starting over

In case you have encountered any problem/error and want to reset the environment to start over, follow the step-by-step instructions below to remove the entire MultiCloud environment:

- [ ] [Google Cloud] Delete VPC Peering
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
​```
- [ ] Clean the Cloud Shell in Google Cloud
```
cd ~
​
rm -rf mission*
​
rm -rf .ssh
```
