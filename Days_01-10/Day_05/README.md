# Day 5: Create an EBS Volume

## 📝 Task Description

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

**Requirements:**
- Create a volume.
- Name of the volume should be `datacenter-volume`.
- Volume **type** must be `gp3`.
- Volume **size** must be `2 GiB`.

---

## ✅ Solution

You can accomplish this task using either the AWS Management Console or the AWS CLI. 

*Note: Amazon EBS Volumes must be created within a specific Availability Zone (AZ). Usually, in these labs, `us-east-1a` is a safe default if no specific AZ is mentioned.*

### Option 1: Using the AWS Management Console (GUI)

1. Open the AWS Management Console and log in with your lab credentials.
2. In the top search bar, search for **EC2** and open the EC2 Dashboard.
3. On the left navigation pane, scroll down to the **Elastic Block Store** section and click on **Volumes**.
4. Click the orange **Create volume** button in the top right.
5. Fill in the volume details:
   - **Volume type**: Select `General Purpose SSD (gp3)` from the dropdown.
   - **Size (GiB)**: Enter `2`.
   - **Availability Zone**: Choose an AZ in your active region (e.g., `us-east-1a`).
6. Scroll down to the **Tags** section and click **Add tag**:
   - **Key**: `Name`
   - **Value**: `datacenter-volume`
7. Click the **Create volume** button at the bottom of the page.

### Option 2: Using the AWS CLI (Terminal)

If you prefer using the terminal, you can run the following command to create the volume and assign its name tag simultaneously:

```bash
aws ec2 create-volume \
  --volume-type gp3 \
  --size 2 \
  --availability-zone us-east-1a \
  --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=datacenter-volume}]'
```
*(If your specific lab environment requires a different AZ, simply change `us-east-1a` to your target AZ).*

---

### 🔍 Verification

To verify that the volume was successfully created with the correct size and type, run:

```bash
aws ec2 describe-volumes \
  --filters Name=tag:Name,Values=datacenter-volume \
  --query "Volumes[*].{ID:VolumeId,Size:Size,Type:VolumeType,State:State}" \
  --output table
```

You should see an output confirming the size is `2` and the type is `gp3`. Once confirmed, you can safely click **Check** in the lab!
