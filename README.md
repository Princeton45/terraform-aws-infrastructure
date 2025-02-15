# Automating AWS Infrastructure with Terraform

This project was a fantastic journey into automating the provisioning of AWS infrastructure using Terraform. I've always been fascinated by the idea of Infrastructure as Code (IaC), and this was my chance to really dive in.

## Project Overview

I set out to create a system that could automatically spin up a complete AWS environment, ready for application deployment. This included the foundational networking components and a compute instance running a Docker container.

## What I Did

1.  **VPC and Networking:** I created the core network, including the VPC, subnets, and routing. I also set up the internet gateway for external access.
  *   *Suggestion for a picture: A diagram showing the VPC structure with subnets, route tables, and the Internet Gateway.*

2.  **EC2 Instance:** I configured an EC2 instance, which is the virtual server where applications will run.
  *   *Suggestion for a picture: The AWS console showing the EC2 instance running, perhaps with its public IP and instance type.*

3.  **Security Group:** I crafted a security group to act as a virtual firewall, controlling the inbound and outbound traffic to the EC2 instance.
  *   *Suggestion for a picture: The AWS console view of the security group rules, highlighting allowed ports.*

4.  **Docker Deployment:** I automated the deployment of a Docker container to the EC2 instance.
    *   *Suggestion for a picture: A terminal showing the `docker ps` command output, displaying the running container on the EC2 instance.*

5. **Version Control**: I used Git to manage all configurations and code.
    *    *Suggestion for a picture: a picture showing a commit history on GitHub*
## Technologies

I used the following technologies:

*   **Terraform:** The heart of the automation. I used Terraform to define and manage the infrastructure as code.
*   **AWS:** The cloud provider where I built everything.
*   **Docker:** For containerizing applications.
*   **Linux:** The operating system for the EC2 instance.
* **Git**: For Version Control.
