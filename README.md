# onpremise-migration-to-multicloud-and-devops

- [x] Part 1: Enable a MultiCloud architecture deployment through Terraform, with resources running in AWS and Google ﻿Cloud Platform
- [x] Part 2: Convert a database and an application to run on the MultiCloud architecture (AWS ﻿and ﻿Google Cloud), including Docker and Kubernetes
- [x] Part 3: Migrate the application files and data from a databas


## Tech Stack



## Part 1

- [x] Create a AWS Free Account
- [x] Create a GCP Free Account
- [x] Create "terraform-en-1" user using the IAM service
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
