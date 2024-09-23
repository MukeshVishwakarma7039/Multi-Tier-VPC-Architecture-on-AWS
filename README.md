# Production Network Setup 🌐

## Four-Tier Architecture Design 🏗️

1. **Presentation Layer**: 
   - **Public Subnet**: **Web**

2. **Application Layer**: 
   - **Private Subnets**: **App1**, **App2**

3. **Caching Layer**: 
   - **Private Subnet**: **Dbcache**

4. **Data Layer**: 
   - **Private Subnet**: **DB**

## Subnet Configuration 🛠️

### Public Subnet
- **Name**: `web`
- **CIDR Block**: `10.0.1.0/24`

### Private Subnets
- **Name**: `app1`
- **CIDR Block**: `10.0.2.0/24`
  
- **Name**: `app2`
- **CIDR Block**: `10.0.3.0/24`
  
- **Name**: `dbcache`
- **CIDR Block**: `10.0.4.0/24`
  
- **Name**: `db`
- **CIDR Block**: `10.0.5.0/24`

## Launch Instances in All Subnets 🚀

- **Web Subnet**: Launch an EC2 instance named `web`
- **App1 Subnet**: Launch an EC2 instance named `app1`
- **App2 Subnet**: Launch an EC2 instance named `app2`
- **Dbcache Subnet**: Launch an EC2 instance named `dbcache`
- **DB Subnet**: Launch an EC2 instance named `db`

## Enable Internet Access 🌍

- Configure a route in the `dbcache` subnet’s route table to allow outbound traffic to the Internet via the Internet Gateway.
- Update the security group for the `dbcache` instance to permit outbound traffic to the Internet (e.g., `0.0.0.0/0`).
- Ensure the `app1` instance can also make Internet requests as needed.

## Manage Security Groups and NACLs 🔒

### Security Groups
- **Web Instance**: Allow inbound traffic on ports 80 (HTTP) and 443 (HTTPS).
- **App1 and App2**: Allow inbound traffic from the web instance's security group.
- **Dbcache**: Allow traffic from `app1` and `app2`.
- **DB**: Allow traffic from `dbcache`.

### NACLs
- **Public Subnet**: Configure NACLs to allow inbound and outbound HTTP/HTTPS traffic.
- **Private Subnets**: Allow inbound traffic from the respective security groups and outbound traffic to the Internet as required.

## Create VPC Endpoint for S3 ☁️

- Go to the VPC Dashboard, select "Endpoints," and create a new endpoint for the S3 service.
- Attach the endpoint policy to allow access to all S3 buckets.
- Ensure that the necessary route tables are updated to direct S3 traffic through the VPC endpoint.

Let’s get this network up and running! 🚀




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
