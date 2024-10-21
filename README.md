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


To push my-app:1.0 imahge to AWS ECR app-repo wehave to follow the instructions:

<img width="797" alt="Screenshot 2024-10-21 at 16 11 22" src="https://github.com/user-attachments/assets/c9f5024c-3ef2-45f5-9f80-cc5344df39bd">


and login into the private repo to authenticate yourself -> use command docker login

![Screenshot 2024-10-21 at 16 17 11](https://github.com/user-attachments/assets/98665e7a-f4aa-4b58-9107-4fae75fd28ca)

in ordet to tell docker I want the image ,y-app:1.0  to be pushed to AWS repo with the name my-app, we tag the image:

![Screenshot 2024-10-21 at 16 20 28](https://github.com/user-attachments/assets/695c995c-1032-4626-9c98-ebda901edd70)


![Screenshot 2024-10-21 at 16 22 25](https://github.com/user-attachments/assets/4469112b-c8ec-417e-979c-4dfbf7f6ae8e)

and now we push the image to the AWS repo:

![Screenshot 2024-10-21 at 16 23 13](https://github.com/user-attachments/assets/0e0f587f-c10c-4ae0-951f-16f08b80a4c5)

now we see the image inside the AWS repo:

<img width="665" alt="Screenshot 2024-10-21 at 16 25 52" src="https://github.com/user-attachments/assets/cd659b0e-038c-4861-a16b-3b40e96e84d8">

14, 16, 17 - Jenkins


