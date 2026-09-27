# Day 2: Create a Security Group

## 📝 Task Description

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.

**Requirements:**
- Create a security group under the default VPC.
- Name of the security group is `nautilus-sg`.
- The description must be `Security group for Nautilus App Servers`.
- Add an inbound rule of type `HTTP`, with port range of `80`. Enter the source CIDR range of `0.0.0.0/0`.
- Add another inbound rule of type `SSH`, with port range of `22`. Enter the source CIDR range of `0.0.0.0/0`.
- Create the resources only in the **`us-east-1`** region.

---

## ✅ Solution

You can accomplish this task using either the AWS Management Console or the AWS CLI.

### Option 1: Using the AWS Management Console (GUI)

1. Open the provided Console URL and log in with your lab credentials.
2. Ensure your active region is set to **`us-east-1` (N. Virginia)** in the top right corner.
3. In the top search bar, search for **EC2** and open the EC2 Dashboard.
4. On the left navigation pane, under **Network & Security**, click on **Security Groups**.
5. Click the orange **Create security group** button.
6. Under **Basic details**, fill in the following:
   - **Security group name**: `nautilus-sg`
   - **Description**: `Security group for Nautilus App Servers`
   - **VPC**: Leave it as the default VPC (it will be pre-selected).
7. Under **Inbound rules**, click **Add rule** twice to create two new rules:
   - **Rule 1**:
     - **Type**: `HTTP`
     - **Port range**: `80` (auto-fills)
     - **Source**: `Anywhere-IPv4` (or Custom with `0.0.0.0/0`)
   - **Rule 2**:
     - **Type**: `SSH`
     - **Port range**: `22` (auto-fills)
     - **Source**: `Anywhere-IPv4` (or Custom with `0.0.0.0/0`)
8. Scroll down to the bottom and click **Create security group**.

### Option 2: Using the AWS CLI (Terminal)

If you prefer using the terminal, you will need to run two separate steps: first to create the empty security group, and second to add the ingress (inbound) rules.

**Step 1: Create the Security Group**
```bash
aws ec2 create-security-group \
  --group-name nautilus-sg \
  --description "Security group for Nautilus App Servers" \
  --region us-east-1
```

**Step 2: Add the Inbound Rules (HTTP and SSH)**
```bash
aws ec2 authorize-security-group-ingress \
  --group-name nautilus-sg \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0 \
  --region us-east-1

aws ec2 authorize-security-group-ingress \
  --group-name nautilus-sg \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0 \
  --region us-east-1
```
