# Day 6: Launch an EC2 Instance

## 📝 Task Description

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.

For this task, create an EC2 instance with the following requirements:
1. The name of the instance must be `devops-ec2`.
2. You can use the **Amazon Linux** AMI to launch this instance.
3. The Instance type must be `t2.micro`.
4. Create a new RSA key pair named `devops-kp`.
5. Attach the default (available by default) security group.
6. Create the resources only in the **`us-east-1`** region.

---

## ✅ Solution

You can accomplish this task using either the AWS Management Console or the AWS CLI.

### Option 1: Using the AWS Management Console (GUI)

1. Open the provided Console URL and log in with your lab credentials.
2. Ensure your active region is set to **`us-east-1` (N. Virginia)** in the top right corner.
3. In the top search bar, search for **EC2** and open the EC2 Dashboard.
4. Click the orange **Launch instance** button.
5. **Name and tags**: Enter `devops-ec2` in the Name field.
6. **Application and OS Images (Amazon Machine Image)**: Select **Amazon Linux** (this is usually the default selection under Quick Start).
7. **Instance type**: Ensure **`t2.micro`** is selected from the dropdown.
8. **Key pair (login)**: 
   - Since you need a new one, click the **Create new key pair** link on the right.
   - **Key pair name**: `devops-kp`
   - **Key pair type**: `RSA`
   - **Private key file format**: `.pem`
   - Click **Create key pair** (this will download the key to your computer, keep it safe!).
9. **Network settings**:
   - Ensure the VPC selected is the **default VPC**.
   - Under Firewall (security groups), choose the **Select existing security group** option.
   - Expand the dropdown and select the **default** security group.
10. Leave all other settings (like storage) at their default values.
11. Click the orange **Launch instance** button on the summary panel on the right.
12. Click on the Instance ID to view it, and wait for the state to change to `running`.

### Option 2: Using the AWS CLI (Terminal)

You uploaded a fantastic and incredibly thorough terminal solution for this task! I reviewed it, and it perfectly covers all requirements (dynamically fetching the Amazon Linux AMI, generating the RSA key pair, mapping the default VPC/SG, and launching).

👉 **[View the Complete Terminal Solution here!](./nautilus_ec2_terminal_solution.md)**
