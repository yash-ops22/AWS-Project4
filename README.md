# awstask4
# About The Task:

Perform task-3 with an additional feature to be added that is NAT Gateway to provide the internet access to instances running in the private subnet.

Performing the following steps:
1.  Write an Infrastructure as code using terraform, which automatically create a VPC.
2.  In that VPC we have to create 2 subnets:
    1.   public  subnet [ Accessible for Public World! ] 
    2.   private subnet [ Restricted for Public World! ]
3. Create a public facing internet gateway for connect our VPC/Network to the internet world and attach this gateway to our VPC.
4. Create  a routing table for Internet gateway so that instance can connect to outside world, update and associate it with public subnet.
5.  Create a NAT gateway for connect our VPC/Network to the internet world  and attach this gateway to our VPC in the public network
6.  Update the routing table of the private subnet, so that to access the internet it uses the nat gateway created in the public subnet
7.  Launch an ec2 instance which has Wordpress setup already having the security group allowing  port 80 sothat our client can connect to our wordpress site. Also attach the key to instance for further login into it.
8.  Launch an ec2 instance which has MYSQL setup already with security group allowing  port 3306 in private subnet so that our wordpress vm can connect with the same. Also attach the key with the same.

Note: Wordpress instance has to be part of public subnet so that our client can connect our site. 
mysql instance has to be part of private  subnet so that outside world can't connect to it.
Don't forgot to add auto ip assign and auto dns name assignment option to be enabled.

# Our Solution

# Step 1:
Before creating any Resources, we have to configure our AWS Profile: Here we have already configure it

     AWS Access Key ID [****************LELI]:
     AWS Secret Access Key [****************Av0P]:
     Default region name [ap-south-1]:   
     Default output format [json]:

Then in our terraform code specifying the provider for creating the resources into the specifide provider...

    provider "aws" {
    region     = "ap-south-1"
    profile    = "Yashu"
    }
    
    
# Step 2:

We have to create a Virtual Private Network or VPC so that we can launch our resurces into it....

Terraform code for our VPC....

    resource "aws_vpc" "cloud4-vpc" {
    cidr_block       = "192.168.0.0/16"
    instance_tenancy = "default"

    tags = {
      Name = "cloud4-vpc"
      }
    }  
    
Here in this step we are creating two subnets one of them is public and the other is private. Private Subnet will store our private resources and data which is not accessible to outside world. Public subnet will contain the resources which will we accessible to outside world for their use.

Public Subnet...    

    resource "aws_subnet" "public-subnet2" {
    vpc_id     = aws_vpc.cloud4-vpc.id
    cidr_block = "192.168.0.0/24"
    map_public_ip_on_launch  = true
    availability_zone = "ap-south-1a"

    tags = {
      Name = "public-subnet2"
        }
      }
      
  Private Subnet

    resource "aws_subnet" "private-subnet1" {
    vpc_id     = aws_vpc.cloud4-vpc.id
    cidr_block = "192.168.1.0/24"
    availability_zone = "ap-south-1a"
     tags = {
      Name = "private-subnet1"
      }
    }    
    
    
