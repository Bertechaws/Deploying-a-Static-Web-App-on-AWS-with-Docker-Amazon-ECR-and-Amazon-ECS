
# Deploying a Static Web App on AWS with Docker, Amazon ECR, and Amazon ECS

This project demonstrates how to deploy a static web app on AWS using Docker, Amazon ECR (Elastic Container Registry), and Amazon ECS (Elastic Container Service). It includes steps to set up the AWS infrastructure, build a Docker container, and deploy the web app in a scalable and secure environment.

## Prerequisites

- **Git**: For version control and repository management.
- **Visual Studio Code**: IDE for coding and building Docker images.
- **Docker**: For creating and managing containerized applications.
- **AWS CLI**: For interacting with AWS services.
- **IAM User**: With permissions to access ECR and ECS.

## Project Resources

- [GitHub Repository](https://github.com/your-repo-url) - Contains the Dockerfile, deployment scripts, and configuration files.
- [Reference Diagram](link-to-diagram) - Diagram illustrating the architecture of the deployment.

## Architecture Overview

### Virtual Private Cloud (VPC)
A VPC is a logically isolated section of the AWS cloud where resources are launched.
- **Internet Gateway**: Facilitates communication between VPC instances and the internet.
- **Security Groups**: Act as virtual firewalls to control inbound and outbound traffic.
- **Availability Zones**: Ensure reliability and fault tolerance by distributing resources.
- **Public Subnets**: Host infrastructure components like the NAT Gateway and Application Load Balancer.
- **Private Subnets**: Host the web servers (EC2 instances) for enhanced security.
- **NAT Gateway**: Allows instances in private subnets to access the internet.
- **Application Load Balancer**: Distributes incoming web traffic across multiple EC2 instances in an Auto Scaling group.
- **AWS Certificate Manager**: Provides SSL/TLS certificates to secure application communications.
- **Simple Notification Service (SNS)**: Sends notifications about activities within the Auto Scaling Group.
- **Route 53**: Manages DNS services for the website's domain name.

### Amazon ECR (Elastic Container Registry)
Amazon ECR is a fully managed container image registry that makes it easy for developers to store, manage, and deploy Docker container images.

### Amazon ECS (Elastic Container Service)
Amazon ECS is a fully managed container orchestration service that enables you to run and scale Docker containers on AWS.

## Deployment Steps

1. **Install Required Tools**
    - Install Git
    - Install Visual Studio Code
    - Install Docker
    - Install AWS CLI

2. **Setup Docker**
    - Sign up for a Docker Hub account.
    - Enable virtualization on your computer.
    - Create a Dockerfile:

      ```Dockerfile
      FROM amazonlinux:latest

      # Install dependencies
      RUN yum update -y && \
          yum install -y httpd && \
          yum search wget && \
          yum install wget -y && \
          yum install unzip -y

      # Change directory
      RUN cd /var/www/html

      # Download web files
      RUN wget https://github.com/azeezsalu/techmax/archive/refs/heads/main.zip

      # Unzip folder
      RUN unzip main.zip

      # Copy files into html directory
      RUN cp -r techmax-main/* /var/www/html/

      # Remove unwanted folder
      RUN rm -rf techmax-main main.zip

      # Expose port 80 on the container
      EXPOSE 80

      # Set the default application that will start when the container starts
      ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]
      ```

3. **Build and Push Docker Image**
    - Build the container image using Visual Studio Code.
    - Start the container locally to test.
    - Create a repository on Docker Hub.
    - Push the Docker image to Docker Hub using:

      ```bash
      docker push <docker-hub-repo-url>
      ```

4. **Setup AWS ECR**
    - Create an ECR repository:

      ```bash
      aws ecr create-repository --repository-name jupiter --region us-east-1
      ```

    - Login to ECR:

      ```bash
      aws ecr get-login-password | docker login --username AWS --password-stdin 438808621720.dkr.ecr.us-east-1.amazonaws.com
      ```

    - Push the Docker image to ECR:

      ```bash
      docker push 438808621720.dkr.ecr.us-east-1.amazonaws.com/jupiter
      ```

5. **Build AWS Infrastructure**
    - Create a three-tier AWS VPC.
    - Set up NAT Gateways and Route Tables.
    - Create Security Groups: Auto Load Balancing and Container Security Group.
    - Set up an Application Load Balancer.
    - Create an ECS Cluster.
    - Define a Task Definition and Service in ECS.

6. **Configure DNS and SSL**
    - Register a domain name in Route 53.
    - Create a Record Set in Route 53.
    - Register an SSL certificate in AWS Certificate Manager.
    - Create an HTTPS listener for the Application Load Balancer.

## Notes

- Ensure that all AWS services are correctly configured with the necessary permissions and security settings.
- Review the AWS costs associated with running ECS, ECR, and other related services.

- ![image](https://github.com/user-attachments/assets/53ad03e7-7a8c-4438-9ea6-cdd40f660de8)


## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contact

For any questions or issues, please contact [Your Name](mailto:your-email@example.com).

---

Feel free to adjust the content according to your specific needs and project details!
