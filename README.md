# Problem Statement 🌐

# 🌐 Designing Multi-Tier Amazon VPCs for Secure Production and Development Environments 🚀

## Project Overview
At **XYZ Corporation**, we are expanding! 🎉 As part of the growth, I’ve been tasked with designing and deploying two **Amazon VPCs**—one for our **production** team and another for **development**. Each environment has specific networking and security requirements, ensuring they function optimally while remaining scalable and secure. 🔒

---

## 🏗️ Production Network Setup

1. **Design a Four-Tier Architecture** 🏢
   - Build a robust 4-tier architecture that separates concerns across different subnets.

2. **Create Five Subnets** 🌐
   - 🟢 Four **private subnets**:
     - `app1`
     - `app2`
     - `dbcache`
     - `db`
   - 🟡 One **public subnet**:
     - `web`

3. **Launch EC2 Instances in Each Subnet** 💻
   - Instances should be named based on the subnet they're in (e.g., `app1` instance for the `app1` subnet).

4. **Allow Internet Access for Specific Instances** 🌍
   - Only the **dbcache instance** and the **app1 subnet** should be able to send internet requests (configure NAT Gateway as needed).

5. **Manage Security Groups & NACLs** 🔐
   - Ensure secure and controlled access to and from each instance and subnet using **Security Groups** and **Network Access Control Lists (NACLs)**.

6. **Create an S3 VPC Endpoint** 🛠️
   - Set up a **VPC Endpoint** for S3 to securely access S3 objects from within the VPC without internet.

---

## 🛠️ Development Network Setup

1. **Design a Two-Tier Architecture** 🏗️
   - Two subnets:
     - `web`
     - `db`

2. **Launch EC2 Instances** 💻
   - Name the instances based on the subnets (`web instance`, `db instance`).

3. **Configure Internet Access for Web Subnet Only** 🌐
   - Ensure that only the **web subnet** can send internet requests while keeping the **db subnet** isolated.

4. **Create a VPC Peering Connection** 🔄
   - Set up a **peering connection** between the production and development VPCs to allow seamless communication.

5. **Setup Communication Between DB Subnets** 🔗
   - Enable secure communication between the **db subnet** in the production network and the **db subnet** in the development network using security rules.

Feel free to customize and expand as per the project’s needs!

Let’s get started! 🚀

### Next Page : https://github.com/MukeshVishwakarma7039/Multi-Tier-VPC-Architecture-on-AWS/tree/Solution
