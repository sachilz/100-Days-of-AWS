# Day 1: Create an EC2 Key Pair

## 📝 Task Description

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.

**Requirements:**
- Create a key pair.
- Name of the **key pair** should be `nautilus-kp`.
- Key pair **type** must be `rsa`.
- Create the resources only in the **`us-east-1`** region.

---

## ✅ Solution

You can accomplish this task using either the AWS Management Console or the AWS CLI.

### Option 1: Using the AWS Management Console

1. Log in to the AWS Management Console using the provided lab credentials.
2. Ensure your active region is set to **`us-east-1` (N. Virginia)** (check the top right corner of the navigation bar).
3. Navigate to the **EC2 Dashboard** by typing "EC2" in the top search bar and selecting it.
4. In the left-hand navigation pane, under the **Network & Security** section, click on **Key Pairs**.
5. Click the orange **Create key pair** button in the top right.
6. Fill in the details exactly as requested:
   - **Name**: `nautilus-kp`
   - **Key pair type**: `RSA`
   - **Private key file format**: Choose `.pem` (for use with OpenSSH/Mac/Linux) or `.ppk` (for use with PuTTY on Windows).
7. Click the **Create key pair** button at the bottom.
8. The key pair will be created, and the private key file will automatically download to your computer.

### Option 2: Using the AWS CLI

If you prefer using the terminal on the AWS client machine (or your own terminal with credentials configured), you can simply run the following command to create the key pair and save the private key locally:

```bash
aws ec2 create-key-pair \
    --region us-east-1 \
    --key-name nautilus-kp \
    --key-type rsa \
    --query "KeyMaterial" \
    --output text > nautilus-kp.pem
```

*Note: Be sure to secure your downloaded `.pem` file by updating its permissions (e.g., `chmod 400 nautilus-kp.pem` on Linux/Mac) so that it isn't publicly readable.*
