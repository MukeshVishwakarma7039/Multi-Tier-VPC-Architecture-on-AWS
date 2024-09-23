## 🚀 Project Outcomes: Multi-Tier Amazon VPCs for Secure Production and Development Environments

### **Production Environment** 🏭

**Step 1:**  
🔐 Log in to the **web instance**, then SSH into the **dbcache instance** from the web instance.  
🌐 Try accessing the **internet** from the dbcache instance.
![image](https://github.com/user-attachments/assets/d0ac6396-e4fb-4c98-81fa-c7857e24defe)

**Step 2:**  
📦 Now, attempt to access an **S3 bucket** from the **dbcache instance** using the VPC Endpoint.  
✅ You will successfully access the S3 bucket **without requiring the internet**.

![s3](https://github.com/user-attachments/assets/bfc21d31-958b-49fe-beaf-62054fcd14a6)
![image](https://github.com/user-attachments/assets/0c43b7d3-c30d-4809-9152-ccfbfa4b1bbc)


**Step 3:**  
🌍 From the **App Server 1** (app1 subnet), try accessing the **internet** through the **NAT Gateway**.

![image](https://github.com/user-attachments/assets/f999ff14-7c6b-4476-935d-fcfda5c69050)


**Outcome:**  
🔒 We have securely accessed the **S3 bucket** without internet, and controlled **internet access** for private subnets through a NAT Gateway.

---

### **Development Environment** 🛠️

**Step 1:**  
🌐 The **web instance** is allowed to access the **internet**, while it can also connect securely to the **DB private instance** in the development network.

![image](https://github.com/user-attachments/assets/868ac292-57c3-41d5-b778-8fb5c5397bd2)

![image](https://github.com/user-attachments/assets/e644090f-bb63-401a-86e2-697c30ea94c1)


**Outcome:**  
💻 The **web instance** in the development VPC can access the internet, and securely communicate with the **DB instance**, keeping internal resources isolated.

---

This project demonstrates how to securely manage internet access and private communication between instances using **Amazon VPC** in both production and development environments. 🛡️

