## To Deploy an EC2 instance using Terraform
provider "aws"{
    region = "us-east-1"
    access_key="XXXXXXXXXXXX"
    secret_key="XXXXX"
}
resource "aws_instance" "my-first-server"{
    ami = "ami-052efd3df9dad4825"
    instance_type = "t2.micro"
}
