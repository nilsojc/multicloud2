<p align="center">
  <img src="assets/diagram.png" 
</p>
  
## ☁️ MultiCloud, DevOps & AI Challenge - Day 2 - Automating AWS provisioning using Terraform  ☁️

This is part of the second project of the challenge/bootcamp! 

In this project we will be 


<h2>Environments and Technologies Used</h2>

  - Amazon Web Services
  - Github Codespaces
  - Amazon Bedrock
  - Docker
  - Kubernetes
  - Elastic Kubernetes Services
  - Elastic Container Registry

  
  
<h2>Features</h2>  





<h2>Step by Step Instructions</h2>

***1. Repo configuration***


NOTE: Keep in mind this is for a Linux environment, check the AWS documentation to install it in your supported OS.


   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install


We then do `AWS configure` and enter our access and secret key along with the region. Output format set to JSON. With this command we will double check that our credentials are put in place for CLI:

```
aws sts get-caller-identity
```

We will then install Kubernetes CLI's which is `eksctl` and `kubectl`:

```
# Download and extract eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

# Move eksctl to /usr/local/bin
sudo mv /tmp/eksctl /usr/local/bin

# Download kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Make it executable and move to /usr/local/bin
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

```

```


***2. Terraform Configuration***

In this step we will make sure that we have terraform set up.

First, we will create a terraform folder and point to it directly:

```
mkdir terraform-project && cd terraform-project
```

Then we will initialize the terraform plaform with the main.tf file. Inside it, is the documentation to create a S3 bucket

```
terraform init
```

Next, we will review the plan:

```
terraform plan
```

Finally, we will apply it.

```
terraform apply
```

We will verify our bucket and dynamoDB table was created successfully with the AWS CLI command:

```
aws s3 ls

```

***3. Final Result***

This will be the S3 bucket and DynamoDB table created with Terraform, as shown in the console:

![image](/assets/image1.png)
![image](/assets/image2.png)

<h2>Conclusion</h2>

In this project, I learned how to leverage terraform to automate creation of AWS services in preparation with the other projects in the multicloud bootcamp!
