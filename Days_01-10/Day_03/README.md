# Day 3: Create a Subnet in the Default VPC

## 📝 Task Description

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.

**Requirements:**
- Create one subnet named `datacenter-subnet` under the default VPC.
- Create the resources only in the **`us-east-1`** region.

---

## ✅ Solution

You can accomplish this task using either the AWS Management Console or the AWS CLI. 

*💡 **Important Note on CIDR Blocks**: Since the task doesn't specify a CIDR block, you must choose one that does not overlap with existing subnets in your default VPC (which typically uses the `172.31.0.0/16` range). Based on your screenshot, existing subnets go up to `172.31.80.0/20`. Therefore, a safe, non-overlapping choice for your new subnet is `172.31.96.0/20`.*

*(For a deep dive into why this works, check out my [CIDR Explanation](./CIDR_Explanation.md) guide!)*

### Option 1: Using the AWS Management Console (GUI)

1. Open the provided Console URL and log in with your lab credentials.
2. Ensure your active region is set to **`us-east-1` (N. Virginia)**.
3. In the top search bar, search for **VPC** and open the VPC Dashboard.
4. On the left navigation pane, click on **Subnets**.
5. Click the orange **Create subnet** button in the top right.
6. **VPC ID**: Select your default VPC from the dropdown.
7. Under **Subnet settings**, configure the following:
   - **Subnet name**: `datacenter-subnet`
   - **Availability Zone**: Choose any available zone (e.g., `us-east-1a`).
   - **IPv4 CIDR block**: Enter a non-overlapping CIDR block (e.g., `172.31.96.0/20`).
8. Click the **Create subnet** button at the bottom.

### Option 2: Using the AWS CLI (Terminal)

If you prefer using the terminal, you first need to grab your Default VPC ID, then create the subnet while applying the name tag in one command.

**Step 1: Get your Default VPC ID**
Run this command to find the ID of your default VPC:
```bash
aws ec2 describe-vpcs \
  --filters "Name=isDefault,Values=true" \
  --query "Vpcs[0].VpcId" \
  --output text
```
*(Assume the output is something like `vpc-0d06d19f26909c5a9`)*

**Step 2: Create the Subnet**
Run the following command, replacing `<YOUR_VPC_ID>` with the ID you retrieved in Step 1:
```bash
aws ec2 create-subnet \
  --vpc-id <YOUR_VPC_ID> \
  --cidr-block 172.31.96.0/20 \
  --region us-east-1 \
  --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=datacenter-subnet}]'
```
