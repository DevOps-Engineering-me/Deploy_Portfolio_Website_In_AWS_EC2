# Deploying a Static Portfolio Web Application on an EC2 Free Tier Instance using Containerization Technology

In this guide, I'll walk you through the steps I took to deploy a static Portfolio web application on an AWS EC2 Free Tier instance using Docker and Nginx. This approach ensures efficient use of limited resources while providing a robust deployment solution.

## Step-by-Step Workflow:

### 1. Pull the Portfolio Application from GitHub
First, clone the repository containing your resume application to your local machine:

```
git clone https://github.com/MdShafiqulSaymon/Resume.git
cd Resume
```

### 2. Write a Dockerfile with Nginx
Create a **Dockerfile** in the root directory of your project to set up Nginx as the web server:

```
# Use the official Node.js image as the base image
FROM node:latest as build

# Set the working directory
WORKDIR /app

# Copy the package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install the dependencies
RUN yarn install --force

# Copy the rest of the application code to the working directory
COPY . .

# Build the React application for production
RUN npm run build

# Use a lightweight web server to serve the static files
FROM nginx:alpine

# Copy the build output to the Nginx html directory
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]

```
### 3. Build the Docker Image

Build the Docker image for your application using the Dockerfile:

```
docker build -t yourusername/resume-app .

```

### 4. Push the Image to Docker Hub

Log in to your Docker Hub account and push the built image:

```
docker login
docker tag yourusername/resume-app yourusername/resume-app:latest
docker push yourusername/resume-app:latest

```

### 5. Set Up EC2 Instance
Launch an EC2 instance using the Free Tier. Connect to your instance using SSH:

```
ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip

```

### 6. Install Docker on EC2

Install Docker on your EC2 instance and log in to Docker Hub:

```
sudo yum update -y
sudo amazon-linux-extras install docker
sudo service docker start
sudo usermod -a -G docker ec2-user
docker login

```

### 7. Pull and Run the Docker Image
Pull the Docker image from Docker Hub and run it with port mapping:

```
docker pull yourusername/resume-app:latest
docker run -d -p 80:80 yourusername/resume-app:latest

```

### 9. Set Up a Custom Domain
Create a subdomain in Namecheap (or your domain registrar) and point it to your EC2 public IP address. Update the DNS settings to create an A record for your subdomain:

I My term:

[mrsaymon.me](http://mrsaymon.me/)
