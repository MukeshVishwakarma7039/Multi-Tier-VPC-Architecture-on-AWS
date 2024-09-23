# VPC Setup Guide 🚀✨

## Production Network 🌐🏗️

### Step 1: Create a VPC
1. Open the [AWS VPC Console](https://console.aws.amazon.com/vpc/). 🔗
2. Click on **Create VPC**. 🆕
3. Choose **VPC and more**.
4. Enter a name for your VPC (e.g., `Production-VPC`) and specify an IPv4 CIDR block (e.g., `10.0.0.0/16`). 🏷️
5. (Optional) Enable IPv6 CIDR block if needed. 🌍
6. Choose **Default** for Tenancy.
7. Click **Create VPC**. 🎉

![image](https://github.com/user-attachments/assets/92792af0-7e95-4750-a519-f719051a5f06)

### Step 2: Create Subnets
1. In the VPC dashboard, select **Subnets** and click **Create subnet**. 🥳
2. Choose your VPC and specify the subnet details:
   - **Public Subnet:** Name: `web`, IPv4 CIDR block: `10.0.1.0/24` 🌞
   - **Private Subnets:** 
     - Name: `app1`, IPv4 CIDR block: `10.0.2.0/24` 🏢
     - Name: `app2`, IPv4 CIDR block: `10.0.3.0/24` 🏢
     - Name: `dbcache`, IPv4 CIDR block: `10.0.4.0/24` 💾
     - Name: `db`, IPv4 CIDR block: `10.0.5.0/24` 📊
3. Click **Create subnet** for each subnet. ✅

![image](https://github.com/user-attachments/assets/5ba4ca2e-eef4-4381-8a02-390f0abf7515)


### Step 3: Create an Internet Gateway
1. In the VPC dashboard, select **Internet Gateways** and click **Create internet gateway**. 🌐
2. Enter a name for the Internet Gateway (e.g., `Production-IGW`) and click **Create internet gateway**. 🎈
3. Attach the Internet Gateway to your VPC:
   - Select the Internet Gateway.
   - Click **Actions** > **Attach to VPC**. 🔄
   - Select your VPC and click **Attach internet gateway**. 🔗

![image](https://github.com/user-attachments/assets/e2133358-7df4-4b05-9f2b-63cd1339e963)


### Step 4: Create a NAT Gateway
1. In the VPC dashboard, select **NAT Gateways** and click **Create NAT gateway**. 🌍
2. Choose the public subnet (`web`) and allocate an Elastic IP. 🖥️
3. Click **Create NAT gateway**. 🛠️

![image](https://github.com/user-attachments/assets/50e1160c-b307-4230-a6ae-a2f7ec327d01)


### Step 5: Configure Route Tables
1. In the VPC dashboard, select **Route Tables** and create two route tables:
   - **Public Route Table:**
     - Click **Create route table**. ✏️
     - Enter a name (e.g., `Public-RT`) and select your VPC.
     - Click **Create route table**. 📋
     - Select the route table, click **Routes** > **Edit routes** > **Add route**. ➕
     - Destination: `0.0.0.0/0`, Target: Internet Gateway. 🌍
     - Click **Save routes**. 💾
     - Click **Subnet associations** > **Edit subnet associations**.
     - Select the `web` subnet and click **Save**. 💡
   - **Private Route Table:**
     - Click **Create route table**. 🖊️
     - Enter a name (e.g., `Private-RT`) and select your VPC.
     - Click **Create route table**. 📃
     - Select the route table, click **Routes** > **Edit routes** > **Add route**. ➕
     - Destination: `0.0.0.0/0`, Target: NAT Gateway. 🔄
     - Click **Save routes**. 💾
     - Click **Subnet associations** > **Edit subnet associations**.
     - Select the `app1`, `app2`, `dbcache`, and `db` subnets and click **Save**. 🚦

![image](https://github.com/user-attachments/assets/9188bcdc-a5fd-4e8d-ab6d-8a22f653de85)


### Step 6: Launch Instances
1. Launch EC2 instances in each subnet and name them according to the subnet names (e.g., `web-instance`, `app1-instance`, etc.). 🚀👩‍💻

![image](https://github.com/user-attachments/assets/d0a8d3dc-17ab-4c60-a6f0-9a4fcb290a54)


### Step 7: Manage Security Groups
1. In the EC2 dashboard, select **Security Groups** and click **Create security group**. 🔐
2. Create security groups for each instance:
   - **Web Security Group:**
     - Name: `web-sg`, VPC: `Production-VPC`. 🌍
     - Inbound rules: Allow HTTP (port 80) and HTTPS (port 443) from anywhere, SSH (port 22) from your IP. 🔒
     - Outbound rules: Allow all traffic. 🌈
   - **App Security Group:**
     - Name: `app-sg`, VPC: `Production-VPC`. 📲
     - Inbound rules: Allow traffic from `web-sg` on the application port (e.g., 8080). 🔄
     - Outbound rules: Allow all traffic. 🌈
   - **DBCache Security Group:**
     - Name: `dbcache-sg`, VPC: `Production-VPC`. 💾
     - Inbound rules: Allow traffic from `app-sg` on the cache port (e.g., 6379). 🔄
     - Outbound rules: Allow all traffic. 🌈
   - **DB Security Group:**
     - Name: `db-sg`, VPC: `Production-VPC`. 📊
     - Inbound rules: Allow traffic from `app-sg` on the database port (e.g., 3306). 🔄
     - Outbound rules: Allow all traffic. 🌈

![image](https://github.com/user-attachments/assets/0006bca2-5328-491a-a4f0-7bd2b1bf9086)


### Step 8: Configure Network ACLs (NACLs)
1. In the VPC dashboard, select **Network ACLs** and create NACLs for each subnet:
   - **Public NACL:**
     - Name: `public-nacl`, VPC: `Production-VPC`. 🌍
     - Inbound rules: Allow HTTP (port 80), HTTPS (port 443), and SSH (port 22) from anywhere. 🔓
     - Outbound rules: Allow all traffic. 🌈
     - Associate with the `web` subnet. 🏗️
   - **Private NACL:**
     - Name: `private-nacl`, VPC: `Production-VPC`. 🔒
     - Inbound rules: Allow traffic from the `web` subnet and other private subnets. 🌍
     - Outbound rules: Allow all traffic. 🌈
     - Associate with the `app1`, `app2`, `dbcache`, and `db` subnets. 🏢

![image](https://github.com/user-attachments/assets/01ef7c09-c4e2-43c1-8934-8d6e6128c614)


### Step 9: Create a VPC Endpoint for S3
1. In the VPC dashboard, select **Endpoints** and click **Create endpoint**. 📦
2. Choose the S3 service and your VPC. 📊
3. Select the route table associated with your private subnets. 🗺️
4. Click **Create endpoint**. 🎉

![image](https://github.com/user-attachments/assets/54f2e4fb-f1f8-48c6-8a8c-134a20bf3d89)


## Development Network 🌱🌍

### Step 1: Create a VPC
1. Open the [AWS VPC Console](https://console.aws.amazon.com/vpc/). 🔗
2. Click on **Create VPC**. 🆕
3. Choose **VPC and more**.
4. Enter a name for your VPC (e.g., `Production-VPC`) and specify an IPv4 CIDR block (e.g., `11.0.0.0/16`). 🏷️
5. (Optional) Enable IPv6 CIDR block if needed. 🌍
6. Choose **Default** for Tenancy.
7. Click **Create VPC**. 🎉

![image](https://github.com/user-attachments/assets/93dbeb2f-6ffb-46f4-900d-12686443d165)


### Step 2: Create Subnets
1. Create two subnets in the development VPC:
   - **Public Subnet:** Name: `web`, IPv4 CIDR block: `11.1.1.0/24` ☀️
   - **Private Subnet:** Name: `db`, IPv4 CIDR block: `11.1.2.0/24` 🏢✅

![image](https://github.com/user-attachments/assets/a63aad84-1264-492b-a697-eaa57b060e17)


### Step 3: Create an Internet Gateway and NAT Gateway
1. Follow the same steps as in the production network to create and attach an Internet Gateway and a NAT Gateway. 🔗

![image](https://github.com/user-attachments/assets/e7fa4446-f8a0-4a03-b3b0-dda7c9f36c87)


### Step 4: Configure Route Tables
1. Create route tables for the development VPC:
   - **Public Route Table:**
     - Associate it with the public subnet (`web`) and add a route to the Internet Gateway. 🌐
   - **Private Route Table:**
     - Associate it with the private subnet (`db`) and add a route to the NAT Gateway. 🛠️

![image](https://github.com/user-attachments/assets/97832c83-479e-4be5-b233-1e9781b4183c)


### Step 5: Launch Instances
1. Launch EC2 instances in each subnet and name them according to the subnet names (e.g., `web-instance`, `db-instance`). 🚀💻

![image](https://github.com/user-attachments/assets/d565d32c-db43-42f4-877a-e9ae4cef4f04)


### Step 6: Manage Security Groups
1. Create security groups for each instance:
   - **Web Security Group:**
     - Name: `dev-web-sg`, VPC: `Development-VPC`. 🌍
     - Inbound rules: Allow HTTP (port 80) and HTTPS (port 443) from anywhere, SSH (port 22) from your IP. 🔒
     - Outbound rules: Allow all traffic. 🌈
   - **DB Security Group:**
     - Name: `dev-db-sg`, VPC: `Development-VPC`. 📊
     - Inbound rules: Allow traffic from `dev-web-sg` on the database port (e.g., 3306). 🔄
     - Outbound rules: Allow all traffic. 🌈

![image](https://github.com/user-attachments/assets/b19f77b9-c139-4c9a-b371-4727e58098e1)


### Step 7: Configure Network ACLs (NACLs)
1. Create NACLs for each subnet:
   - **Public NACL:**
     - Name: `dev-public-nacl`, VPC: `Development-VPC`. 🌍
     - Inbound rules: Allow HTTP (port 80), HTTPS (port 443), and SSH (port 22) from anywhere. 🔓
     - Outbound rules: Allow all traffic. 🌈
     - Associate with the `web` subnet. 🏗️
   - **Private NACL:**
     - Name: `dev-private-nacl`, VPC: `Development-VPC`. 🔒
     - Inbound rules: Allow traffic from the `web` subnet. 🌍
     - Outbound rules: Allow all traffic. 🌈
     - Associate with the `db` subnet. 🏢

### Step 8: Create a Peering Connection (Continued)
1. In the VPC dashboard, select **Peering Connections** and click **Create peering connection**. 🔗
2. Choose the **Requester VPC** (Production) and the **Accepter VPC** (Development). 🌉
3. Click **Create peering connection** and accept the request in the Accepter VPC. ✅
4. Update the route tables in both VPCs to allow traffic between the VPCs:
   - **Production VPC Route Table:**
     - Select the route table associated with the private subnets.
     - Click **Routes** > **Edit routes** > **Add route**. ➕
     - Destination: CIDR block of the Development VPC (e.g., `10.1.0.0/16`), Target: Peering Connection. 🌍
     - Click **Save routes**. 💾
   - **Development VPC Route Table:**
     - Select the route table associated with the private subnets.
     - Click **Routes** > **Edit routes** > **Add route**. ➕
     - Destination: CIDR block of the Production VPC (e.g., `10.0.0.0/16`), Target: Peering Connection. 🌍
     - Click **Save routes**. 💾
    
![image](https://github.com/user-attachments/assets/1a3d1cbb-fbf4-4e0c-83f4-466afc61b41c)

![image](https://github.com/user-attachments/assets/ad61df81-ced8-48e8-ae1e-6b1698e792f5)

![image](https://github.com/user-attachments/assets/e9c0b1a2-fa92-47ab-aa3f-2e3d080fca8c)


### Step 9: Setup Connection Between DB Subnets
1. **Update Security Groups:**
   - In the EC2 dashboard, select **Security Groups** and update the inbound rules for the `db-sg` in the Production VPC and `dev-db-sg` in the Development VPC to allow traffic from each other. 🔄
     - **Production DB Security Group (`db-sg`):**
       - Inbound rule: Allow traffic from the `dev-db-sg` on the database port (e.g., 3306). 📊
     - **Development DB Security Group (`dev-db-sg`):**
       - Inbound rule: Allow traffic from the `db-sg` on the database port (e.g., 3306). 🔄
2. **Update Route Tables:**
   - Ensure that the route tables in both VPCs have routes to each other's CIDR blocks via the peering connection, as described in Step 8. 📈
  
![image](https://github.com/user-attachments/assets/0045ea24-7126-4548-bcad-6cf5767d3497)

![image](https://github.com/user-attachments/assets/2b267c35-86c4-4e2d-9706-184f5a698839)

## Additional Tips 📝💡

- **IP Address Planning:** Ensure that the IP address ranges for the subnets do not overlap to avoid routing conflicts. 🔍
- **Monitoring and Logging:** Use AWS CloudWatch and VPC Flow Logs to monitor and log network traffic for security and troubleshooting purposes. 📊🔍
- **Testing Connectivity:** After setting up the peering connection and updating the route tables and security groups, test the connectivity between instances in the `db` subnets of both VPCs to ensure they can communicate with each other. 🛠️🔗

## Summary 🏁🎊

By following these steps, you will have set up distinct Amazon VPCs for the Production and Development networks with the required subnets, instances, security groups, NACLs, and peering connections. This setup ensures a secure and efficient network architecture for your company's expansion requirements. 🌟✨

If you have any more questions or need further assistance with any specific step, feel free to ask! 💬🤗

### Next Page : https://github.com/MukeshVishwakarma7039/Multi-Tier-VPC-Architecture-on-AWS/tree/Outcome
