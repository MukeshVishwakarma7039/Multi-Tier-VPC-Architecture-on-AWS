# Problem Statement 🌐

You’re tasked with creating and setting up distinct Amazon VPCs for the production and development teams at XYZ Corporation. Here’s what needs to be done:

## Production Network 🏢

1. **Design and Build a Four-Tier Architecture**
2. **Create Five Subnets**: 
   - Four private subnets: **app1**, **app2**, **dbcache**, and **db**
   - One public subnet: **web**
3. **Launch Instances**: Deploy instances in all subnets, naming them according to their respective subnet names.
4. **Internet Access**: Allow the `dbcache` instance and the `app1` subnet to send Internet requests.
5. **Manage Security Groups and NACLs** 🔒
6. **VPC Endpoint for S3**: Create an endpoint for the S3 service, enabling access to objects in any bucket from within the VPC.

## Development Network 🚀

1. **Design and Build a Two-Tier Architecture**
2. **Create Two Subnets**: 
   - Subnets named **web** and **db**, with instances launched and named accordingly.
3. **Internet Access**: Ensure only the **web** subnet can send Internet requests.
4. **Peering Connection**: Establish a peering connection between the production and development networks.
5. **Database Subnet Connection**: Set up a connection between the `db` subnets of both the production and development networks.

Let’s get started! 🚀