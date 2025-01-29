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

***2. Create Docker Image for CloudMart and configure Backend and Frontend***

In this step we will be creating our docker image for CloudMart where we will grab the source code and generate the files.

We will begin by creating the Backend folder and downloading the source code:

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

Next, set the Dockerfile for the backend:

```
nano Dockerfile

FROM node:18
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```
Then, we will create the folder for the frontend and download the source code:

```
cd ..
mkdir frontend && cd frontend
wget https://tcb-public-events.s3.amazonaws.com/mdac/resources/day2/cloudmart-frontend.zip
unzip cloudmart-frontend.zip
```

Lastly, we will setup our Dockerfile for our frontend Docker Image:

```
nano Dockerfile
```

```
#Content of the FrontEnd Dockerfile

FROM node:16-alpine as build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:16-alpine
WORKDIR /app
RUN npm install -g serve
COPY --from=build /app/dist /app
ENV PORT=5001
ENV NODE_ENV=production
EXPOSE 5001
CMD ["serve", "-s", ".", "-l", "5001"]
```


***3. Kubernetes Cluster Creation***

In this step we will make sure t

***4. Kubernetes Backend Deployment***

This will be the S3 bucket and DynamoDB table created with Terraform, as shown in the console:

![image](/assets/image1.png)
![image](/assets/image2.png)


***8. Cleanup***

Once we are done with the project we can proceed with cleaning up the resources:

```
kubectl delete service cloudmart-frontend-app-service
kubectl delete deployment cloudmart-frontend-app
kubectl delete service cloudmart-backend-app-service
kubectl delete deployment cloudmart-backend-app

eksctl delete cluster --name cloudmart --region us-east-1
```

<h2>Conclusion</h2>
