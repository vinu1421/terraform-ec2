# Deploying EC2 using terraform

 Terraform is an open-source infrastructure as code software tool used to define and provision a datacenter infrastructure using a high-level configuration language.

 You can install terraform by downloading it and copying to /bin directory.
 Markup : 1. wget https://releases.hashicorp.com/terraform/0.12.16/terraform_0.12.16_linux_amd64.zip
          2. unzip  terraform_0.12.16_linux_amd64.zip
          3. cp -pr terraform /bin
          4. terraform --version

 The configuration should ends with an extension .tr



## **code.tr**
```
provider "aws" {

  access_key = "youraccesskey"
  secret_key = "yoursecretaccess"
  region = "us-east-1"
}

resource "aws_key_pair" "sshkey" {
  key_name = "vinu-key"
  public_key = "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDVf/gk1lhuq3YbdREMeUHaA9lLu+c8JJdsnmAXoUaTqX/dBAm0KOdkb7cMcnv8WNrMh9r9ePOlWw9NqWos9yjM2lt5IN2ZudcbnXZnQG1K6nmCkvcItvNm62BTRTpUntDEWnatilJHILsLJmv19ujieKUQtpQWJwkQ/lDvj4IiMOwowHaQbWCBp9yTxv58G6yA38wJZNsRLfduwrQeXW+z3pyCGSUQWR99C1aQyJu2JLLe89nOhOx25covcouBunOFamhxHuyMwISj+ETGeWQCxx/5upOdQHC5jSnCxeVcBRwWq/NVNYOJTWtzUuDmOoG0986BGB3SHVGyUnIQhiel root@ip-172-31-22-159.ec2.internal"
}

resource "aws_security_group" "terraform-security"{

        name = "terraform-security"
        description = "allowing port 22 and 80 from all"

        ingress {

           cidr_blocks = ["0.0.0.0/0"]
           from_port   = 22
           to_port     = 22
           protocol    = "tcp"
        }

        ingress {

          cidr_blocks = ["0.0.0.0/0"]
          from_port   = 80
          to_port     = 80
          protocol    = "tcp"
        }

        egress {

          from_port   = 0
          to_port     = 0
          protocol    = -1
          cidr_blocks  = ["0.0.0.0/0"]
        }

        tags = {

          name = "terraform-security"
        }

   }


resource "aws_instance" "terraform-ec2"{

        ami = "ami-00068cd7555f543d5"
        instance_type = "t2.micro"
        key_name = "vinu-key"
        availability_zone = "us-east-1a"
        associate_public_ip_address = true
        vpc_security_group_ids = [ "${aws_security_group.terraform-security.id}" ]
        tags = {
          Name = "test-server"
                }
}

````

To Execute
```
terraform validate - To check the syntax

terraform plan - To dry run (To know what changes are going to take place)

terraform apply - To provision 

terraform destroy - To remove everything
```


