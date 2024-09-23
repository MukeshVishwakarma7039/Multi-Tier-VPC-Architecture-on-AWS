# Development Network Setup 🚀

## Two-Tier Architecture Design 🏗️

1. **Presentation Layer**: 
   - **Public Subnet**: **Web**

2. **Data Layer**: 
   - **Private Subnet**: **DB**

## Subnet Configuration 🛠️

### Public Subnet
- **Name**: `web`
- **CIDR Block**: `10.0.6.0/24`

### Private Subnet
- **Name**: `db`
- **CIDR Block**: `10.0.7.0/24`

## Launch Instances in Both Subnets 🚀

- **Web Subnet**: Launch an EC2 instance named `web`
- **DB Subnet**: Launch an EC2 instance named `db`

## Enable Internet Access 🌍

- Configure the route table for the `web` subnet to allow outbound traffic to the Internet via the Internet Gateway.
- Ensure that the `db` subnet does not have outbound Internet access by default.

## Create VPC Peering Connection 🔗

- Establish a VPC peering connection between the production VPC and the development VPC.
- Update the route tables in both VPCs to enable traffic flow between them. Ensure security groups are configured to allow traffic as needed.

## Setup Connection Between DB Subnets 🔄

- Modify the security groups of the `db` instances in both networks to permit inbound traffic from each other’s subnets.
- Update the route tables to allow direct communication between the `db` subnets of the production and development networks.

Let’s get the development network up and running! 🚀