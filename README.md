# Complete CI/CD Pipeline with EKS and AWS ECR

Complete CI/CD Pipeline with EKS and AWS ECR
Technologies used:
Kubernetes, Jenkins, AWS EKS, AWS ECR, Java, Maven, Linux, Docker, Git
Project description:
- Create private AWS ECR Docker repository
- Adjust Jenkinsfile to build and push Docker Image to AWS ECR
- Integrate deploying to K8s cluster in the CI/CD pipeline from AWS ECR private registry
- So the complete CI/CD project we build has the following configuration:
a. CI step: increment version
b. CI step: Build artifact for Java Maven application
c. CI step: Build and push Docker image to AWS ECR
d. CD. Step: Deploy new application version to EKS cluster
e. CD step: Commit the version update


# Create private AWS ECR Docker repository

We go to ECR (Elastic Container Registry) in AWS and create a repository, in my case I named it ana-repo.

<img width="604" alt="Screenshot 2024-10-21 at 15 13 52" src="https://github.com/user-attachments/assets/56ddb512-17a2-430b-9a97-428889058a1f">

You create a Docker repo per image. Inside the repo you have a collection of related images with same name but different versions. Ex:

![Screenshot 2024-10-21 at 15 20 42](https://github.com/user-attachments/assets/bd330562-5da6-4871-925f-d0e75f3b1e32)

Integrate deploying to K8s cluster in the CI/CD pipeline from AWS ECR private registry

We use the files from my-app (attached) and build an image with command:

```bash
docker build -t my-app:1.0 .
```
![Screenshot 2024-10-21 at 15 45 45](https://github.com/user-attachments/assets/7febfddd-ba55-472f-a7a6-14b6b0249c65)

we can see the image now with command:

```bash
docker images
```

![Screenshot 2024-10-21 at 15 47 34](https://github.com/user-attachments/assets/64ca3d9e-6336-4228-bc7c-a5a866855368)

we run the app:

![Screenshot 2024-10-21 at 15 53 38](https://github.com/user-attachments/assets/90847dac-05f6-4883-8c7c-a887d8bb5377)

our app is running:

![Screenshot 2024-10-21 at 15 54 06](https://github.com/user-attachments/assets/e69b42ac-055d-4a8e-86fd-a09918514119)

we can see the logs:

![Screenshot 2024-10-21 at 15 55 09](https://github.com/user-attachments/assets/f9f6363b-feaf-4677-84bd-14b99b1715bd)

let's get the command line terminal of the container and look:

![Screenshot 2024-10-21 at 15 57 11](https://github.com/user-attachments/assets/0b242808-21a6-4292-8ecb-8d8668dabc74)

here we get the MONGO_DB_USERNAME and password:

![Xnip2024-10-21_16-02-37](https://github.com/user-attachments/assets/7e17de31-0ddf-46b9-9cb7-6cd93a61c70c)


To push my-app:1.0 image to AWS ECR app-repo wehave to follow the instructions:

<img width="797" alt="Screenshot 2024-10-21 at 16 11 22" src="https://github.com/user-attachments/assets/c9f5024c-3ef2-45f5-9f80-cc5344df39bd">


and login into the private repo to authenticate yourself -> use command docker login

![Screenshot 2024-10-21 at 16 17 11](https://github.com/user-attachments/assets/98665e7a-f4aa-4b58-9107-4fae75fd28ca)

in ordet to tell docker I want the image my-app:1.0  to be pushed to AWS repo with the name my-app, we tag the image:

![Screenshot 2024-10-21 at 16 20 28](https://github.com/user-attachments/assets/695c995c-1032-4626-9c98-ebda901edd70)


![Screenshot 2024-10-21 at 16 22 25](https://github.com/user-attachments/assets/4469112b-c8ec-417e-979c-4dfbf7f6ae8e)

and now we push the image to the AWS repo:

![Screenshot 2024-10-21 at 16 23 13](https://github.com/user-attachments/assets/0e0f587f-c10c-4ae0-951f-16f08b80a4c5)

now we see the image inside the AWS repo:

<img width="665" alt="Screenshot 2024-10-21 at 16 25 52" src="https://github.com/user-attachments/assets/cd659b0e-038c-4861-a16b-3b40e96e84d8">


# Adjust Jenkinsfile to build and push Docker Image to AWS ECR

we install Jenkins with a  Digital Ocean droplet, a server. We take 4GB of CPU:

![Screenshot 2024-10-21 at 17 43 53](https://github.com/user-attachments/assets/d95e1e78-b873-4c09-9cf8-4c177911ad67)


we attach a firewall to it with port 22 (ssh) and 8080 (whee Jenkins app starts and where we will expose it):

![Screenshot 2024-10-21 at 17 06 14](https://github.com/user-attachments/assets/512746e6-fa03-4437-9121-4aa6111d4e9a)


we do ssh into the jenkins-server:

![Screenshot 2024-10-21 at 17 44 21](https://github.com/user-attachments/assets/064da874-4128-495d-b775-091eff1e8d9e)

we install docker:

![Screenshot 2024-10-21 at 17 49 08](https://github.com/user-attachments/assets/ef7a0e96-ba7c-478d-8ffb-73334bdac602)

once installed, we can run Jenkins:

![Screenshot 2024-10-21 at 17 56 51](https://github.com/user-attachments/assets/5e4b0ed7-1fe1-41fb-82eb-061d3e301310)

we check:

![Screenshot 2024-10-21 at 17 57 11](https://github.com/user-attachments/assets/150fb02f-9e3c-4a47-88ad-3ead005f5481)


now we can access jenkins from the browser,  copy the ip from the jenkins-server droplet and add port 8080:

![Screenshot 2024-10-21 at 17 59 15](https://github.com/user-attachments/assets/47adb2a3-1c4c-435f-9503-ce6627a72cf0)

the initial password is located in /var/jenkins_home/secrets/initialAdminPassword. we enter the Docker container and cat the content inside /var/jenkins_home/secrets/initialAdminPassword. This is the password we put in Jenkins.

![Screenshot 2024-10-21 at 18 01 21](https://github.com/user-attachments/assets/bfa7482e-e944-401a-befd-45eb53ea1e3d)

we go to the suggested plugins and create our user and access Jenkins!

![Screenshot 2024-10-21 at 18 05 08](https://github.com/user-attachments/assets/c26ea2ff-7458-4112-aeeb-789f205c9e3c)

# Integrate deploying to K8s cluster in the CI/CD pipeline from AWS ECR private registry


# Complete CI/CD project we build has the following configuration:
a. CI step: increment version
c. CI step: Build and push Docker image to16 AWS ECR
d. CD. Step: Deploy new application version to EKS cluster
e. CD step: Commit the version update





