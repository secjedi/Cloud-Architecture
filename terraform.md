## To Deploy an EC2 instance using Terraform
provider "aws"{<br/>
    region = "us-east-1"<br/>
    access_key="XXXXXXXXXXXX"<br/>
    secret_key="XXXXX"<br/>
}<br/>
resource "aws_instance" "my-first-server"{<br/>
    ami = "ami-052efd3df9dad4825"<br/>
    instance_type = "t2.micro"<br/>
    tags = {<br/>
        Name = "Ubuntu"<br/>
    }<br/>
}<br/>

<br/> $terraform apply <br/>
$terraform destroy
