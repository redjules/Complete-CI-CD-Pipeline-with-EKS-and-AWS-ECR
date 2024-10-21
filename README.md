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
c. CI step: Build and push Docker image to16 AWS ECR
d. CD. Step: Deploy new application version to EKS cluster
e. CD step: Commit the version update


# Create private AWS ECR Docker repository
video 10, 11 - Docker
We go to ECR (Elastic Container Regostry) in AWS and create a repository, in my case I named it ana-repo.

<img width="604" alt="Screenshot 2024-10-21 at 15 13 52" src="https://github.com/user-attachments/assets/56ddb512-17a2-430b-9a97-428889058a1f">

You create a Docker repo per image. Inside the repo you have a collection of related images with same name but different versions. Ex:

![Screenshot 2024-10-21 at 15 20 42](https://github.com/user-attachments/assets/bd330562-5da6-4871-925f-d0e75f3b1e32)


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

14, 16, 17 - Jenkins


