# Project README

This README file provides an overview of the project and its components. It covers the steps taken to dockerize a Go application, minimize its weight, establish a connection to a MySQL container, integrate Jenkins for continuous integration, implement high availability using HPA (Horizontal Pod Autoscaler), and integrate the Helm chart with ArgoCD. It also includes relevant screenshots showcasing the project setup.

## Project Overview

The objective of this project is to containerize a Go application using Docker, optimize its size, establish a connection to a MySQL container, integrate Jenkins for continuous integration, ensure high availability through HPA, and utilize Helm charts and ArgoCD for efficient deployment and synchronization.

## Project Structure

The project structure is organized as follows:

- internship/
  - Dockerfile           # Dockerfile for Go application
  - docker-compose.yml   # Docker Compose file for connecting to MySQL container
  - kubernetes/           # contain database and goappfiles
- jenkins/
  - Jenkinsfile           # Jenkins Dockerfile
- helmchart/
  - instabugchart/        # Helm chart for the application

## Dockerization Steps

1. **Dockerizing the Go App**: The Go application is containerized using Docker, following best practices defined in the Dockerfile.

2. **Optimizing Image Size**: Techniques are employed to minimize the weight of the Docker image, reducing its size and improving efficiency.

3. **Connecting to MySQL Container**: The docker-compose.yml file is created to establish a connection between the Go application container and a MySQL container.

![Screenshot 1: Connection Establishment](/screens/screen1compose.png)

![Screenshot 2: Connection Establishment](/screens/screen2compose.png)

![Screenshot 3: Connection Establishment](/screens/dbscreen.png)

![Screenshot 4: Kubernetes Deployment ](/screens/k8s1.png)

![Screenshot 5: argocd and DB screen ](/screens/screens/helm.png)

![Screenshot 6: ArgoCD Integration (1/2) ](/screens/argo1.png)

![Screenshot 7: ArgoCD Integration (2/2)](/creens/argo2.png)

## Jenkins Integration

1. **Installing Jenkins**: Jenkins is installed within a container, utilizing a custom Jenkins image.

2. **Mounting Docker Socket Volume**: The Docker socket volume is mounted to enable the execution of Docker commands on the host machine.

3. **Jenkins Pipeline Script**: The Jenkinsfile contains a pipeline script that consists of three major steps:
   - Building the Docker image
   - Pushing the Docker image to a repository
   - Committing changes to the Helm repository with a new version number
   - Sending to slackchanel

## High Availability with HPA

1. **Implementing Horizontal Pod Autoscaler (HPA)**: An HPA configuration file (hpa.yml) is provided to enable automatic scaling of the application based on resource utilization.
## Creating Database StatefulSet with Pv and Pvc
1. I used kind Statefulset wich is good choice for statefull app in k8s world
2. I use pvc as storage backend and depend on hostpath wihch is good for localtesting in minikube
3. use pvc and attached it to storage of database to presist the data


## Helm Chart Integration with ArgoCD

1. **Creating Helm Chart**: A Helm chart named "instabugchart" is generated using the command `helm create instabugchart` within the "helmchart" directory.

2. **Integrating Helm Chart with ArgoCD**: The Helm chart is integrated with ArgoCD to enable continuous integration and deployment. ArgoCD syncs the deployment with the latest version of the Helm chart when triggered by Jenkins.

## Usage

To use this project, follow these steps:

1. Dockerize the Go application using the provided Dockerfile.
2. Configure the docker-compose.yml file to establish the connection to the MySQL container.
3. Set up Jenkins using the custom Jenkins image and mount the Docker socket volume.
4. Create a Jenkins pipeline job using the Jenkinsfile and configure Slack notifications.
5. Implement the HPA configuration by applying the hpa.yml file to your Kubernetes cluster.
6. Generate the Helm chart for your application using `helm create instabugchart` within the "helmchart" directory.
7. we can install with `helm install instachart helmchart/instabugchart`
7. Integrate the Helm chart with ArgoCD for continuous integration and deployment.
