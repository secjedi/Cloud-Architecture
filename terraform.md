## To Deploy an EC2 instance using Terraform
provider "aws"{ 
    region = "us-east-1"
    access_key="XXXXXXXXXXXX"
    secret_key="XXXXX"
}<br/>
resource "aws_instance" "my-first-server"{
    ami = "ami-052efd3df9dad4825"
    instance_type = "t2.micro"
    tags = 
        Name = "Ubuntu"
    }
}

<br/> $terraform apply
$terraform destroy

###VPC and Subnets

resource "aws_vpc" "business-vpc" {
  cidr_block       = "10.0.0.0/16"

  tags = {
    Name = "Business vpc Unit"
  }
}

resource "aws_subnet" "Business-subnet-1" {
  vpc_id     = aws_vpc.business-vpc.id
  cidr_block = "10.0.1.0/24"

  tags = {
    Name = "Business-subnet"
  }
}
