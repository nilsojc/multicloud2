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

***2. Create Docker Image for CloudMart***

In this step we will be creating our docker image for CloudMart where we will grab the source code and generate the files.

First, we will create the Bedrock ID and Bedrock Agent alias ID:

```
aws bedrock-agent create-agent \
    --agent-name <agent-name> \
    --foundation-model <foundation-model> \
    --role-arn <role-arn> \
    --description "My Bedrock Agent"
```

```
aws bedrock-agent create-agent-alias \
    --agent-id <agent-id> \
    --agent-alias-name <agent-alias-name> \
    --description "My Agent Alias"
```

We can verify them with these commands:

```
aws bedrock-agent list-agents
aws bedrock-agent list-agent-aliases --agent-id <agent-id>
```

We will then be creating the folder and downloading source code for the Back End:

```
mkdir -p challenge-day2/backend && cd challenge-day2/backend
wget https://tcb-public-events.s3.amazonaws.com/mdac/resources/day2/cloudmart-backend.zip
unzip cloudmart-backend.zip
```

We then create an .env file and provide with the following:

```
nano .env

PORT=5000
AWS_REGION=us-east-1
BEDROCK_AGENT_ID=<your-bedrock-agent-id>
BEDROCK_AGENT_ALIAS_ID=<your-bedrock-agent-alias-id>
OPENAI_API_KEY=<your-openai-api-key>
OPENAI_ASSISTANT_ID=<your-openai-assistant-id>
```

Lastly, we will setup our Dockerfile for our Docker Image:

```
nano Dockerfile
```

```
#Content of the Dockefile
FROM node:18
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

***3. Kubernetes Cluster Creation***

In this step we will make sure t

***4. Kubernetes Backend Deployment***

This will be the S3 bucket and DynamoDB table created with Terraform, as shown in the console:

![image](/assets/image1.png)
![image](/assets/image2.png)

<h2>Conclusion</h2>

In this project, I learned how to leverage terraform to automate creation of AWS services in preparation with the other projects in the multicloud bootcamp!
