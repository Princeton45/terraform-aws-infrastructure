# Automating AWS Infrastructure with Terraform

This project was a fantastic journey into automating the provisioning of AWS infrastructure using Terraform. I've always been fascinated by the idea of Infrastructure as Code (IaC), and this was my chance to really dive in.

## Project Overview

I set out to create a system that could automatically spin up a complete AWS environment, ready for application deployment. This included the foundational networking components and a compute instance running a Docker container.

![diagram](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/diagram.png)

## Technologies

I used the following technologies:

*   **Terraform:** The heart of the automation. I used Terraform to define and manage the infrastructure as code.
*   **AWS:** The cloud provider where I built everything.
*   **Docker:** For containerizing applications.
*   **Linux:** The operating system for the EC2 instance.
* **Git**: For Version Control.


## What I Did

1.  **VPC and Networking:** I created the core network, including the VPC, subnets, and routing. I also set up the internet gateway for external access.

![vpc](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/vpc.png)

![subnet](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/subnet.png)

![igw](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/igw.png)

Additionally, I created a new route table that includes a rule for internet traffic using the Internet Gateway (by default when you create a VPC that isn't the default VPC, it does not create a rule for internet traffic.)

![rtb](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/rtb.png)


2.  **Security Group:** I created a security group to act as a virtual firewall for the EC2 instance. I opened port 22 for SSH and 8080 so that I can connect to the Nginx docker container from the web browser.

![sg](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/sg.png)


3.  **EC2 Instance:** I configured an EC2 instance, which is the virtual server where applications will run. I made sure to associate the subnet, vpc security group and availability zone that I created earlier to the EC2 instance.

![ec2](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/ec2.png)

Additionally, I automated the creation of the ssh key pair for the EC2 instance with Terraform so we don't have to manually create it.

```hcl
resource "aws_key_pair" "ssh-key" {
  key_name = "server-key"
  public_key = file(var.public_key_location)
}
```

Then I associated that key pair to the EC2 Instance

`key_name = aws_key_pair.ssh-key.key_name`

```hcl
resource "aws_instance" "myapp-server" {
  ami = data.aws_ami.latest-amazon-linux-image.id
  instance_type = var.instance_type

  subnet_id = aws_subnet.myapp-subnet-1.id
  vpc_security_group_ids = [aws_security_group.myapp-sg.id]
  availability_zone = var.avail_zone

  associate_public_ip_address = true
  key_name = aws_key_pair.ssh-key.key_name

  tags = {
    Name: "${var.env_prefix}-server"
  }
  ```


4.  **Docker Deployment:** I automated the deployment of a Docker container to the EC2 instance.

I used AWS `user-data` to install and configure Docker on the EC2 instance and configured it so that if the user-data script changes, it will automatically recreate the EC2 instance.

```hcl
  user_data = file("entry-script.sh")
  user_data_replace_on_change = true
```

Now the docker image is running on the EC2 instance and can be accessed at port 8080

![docker](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/docker.png)

![8080](https://github.com/Princeton45/terraform-aws-infrastructure/blob/main/images/8080.png)
