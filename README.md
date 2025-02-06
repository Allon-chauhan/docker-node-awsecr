# 🐳 Dockerizing Node.js Application & Pushing to a Private Docker Repository

### 🛠️ Technologies Used:
1. Docker
2. Node.js
3. Amazon ECR

## ✅ Prerequisites:
1. AWS account created.
2. AWS CLI installed and configured with access key & secret access key.
3. Downloaded sample Node.js application from [here](https://gitlab.com/twn-devops-bootcamp/latest/07-docker/js-app.git).

### 📜 Writing and Configuring Dockerfile
1. In any directory, create a file named **Dockerfile**.
2. Use the latest Node image.
3. Input the following in the Dockerfile:
   ```dockerfile
   FROM node:latest
   
   ENV MONGO_DB_USERNAME=admin \
       MONGO_DB_PWD=password
   
   RUN mkdir -p /home/node-app
   
   COPY ./app /home/node-app
   
   WORKDIR /home/node-app
   
   RUN npm install
   
   CMD ["node", "server.js"]
   ```

### 🔨 Building Docker Image
1. Ensure the **Dockerfile** is saved in a directory where the `app` folder is available.
2. Run the following command to build the Docker image:
   ```bash
   docker build -t my-app:1.1.0 .
   ```

### 🚀 Pushing the Docker Image to AWS ECR
1. **Create a repository** in AWS ECR.
2. **Authenticate Docker** with AWS ECR using the "push commands" available in the AWS ECR portal:
   ```bash
   aws ecr get-login-password --region <aws-region> | \
   docker login --username AWS --password-stdin <aws-ecr-repository-number>.dkr.ecr.<aws-region>.amazonaws.com
   ```
   ⚠️ **IMPORTANT:** 🚨 Authentication is required before pushing images to AWS ECR.🚨

3. **Tag the Docker image** before pushing:
   ```bash
   docker tag my-app:1.1.0 <aws-ecr-repository-number>.dkr.ecr.<aws-region>.amazonaws.com/my-app:1.1.0
   ```
4. **Push the tagged Docker image** to the AWS ECR repository:
   ```bash
   docker push <aws-ecr-repository-number>.dkr.ecr.<aws-region>.amazonaws.com/my-app:1.1.0
   ```

⚠️ **IMPORTANT:** 🚨 Ensure your AWS credentials and IAM user permissions are correctly configured to avoid authentication errors! 🚨
