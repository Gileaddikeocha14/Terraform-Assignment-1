ASSIGNMENT:
Using Terrafrom create an aws instance (ubuntu) in multiple regions (free-tier) (eu-west-1 and eu-central-1)

- should be on minimum of 2 availablity zones

- should be reuseable

- can be built on multiple environments (dev, prod)

- should have a script that creates ansible, docker container

- your scripts should be modularized

- Create a VPC for the various environments
Dev environment in one region ,production environment in another region>>>



# creating instance 1

resource "aws_instance" "instance" {
  ami               = var.ami
  instance_type     = var.type
  key_name          = var.key_pair
  security_groups   = [aws_security_group.security_group.id]
  subnet_id         = aws_subnet.public_subnet_az1.id
  availability_zone = data.aws_availability_zones.available_zones.names[0]

    user_data = <<-EOF
              #!/bin/bash
              sudo apt-get update -y
              sudo apt-get install docker.io -y
              sudo systemctl start docker
              sudo systemctl enable docker
              sudo apt install ansible -y
              EOF

  tags = {
    Name   = "${var.project_name}-instance"
    source = "terraform"
  }
}
