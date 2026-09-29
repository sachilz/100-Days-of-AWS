# Day 4: Enable Versioning on an S3 Bucket

## 📝 Task Description

Data protection and recovery are fundamental aspects of data management. It's essential to have systems in place to ensure that data can be recovered in case of accidental deletion or corruption. The DevOps team has received a requirement for implementing such measures for one of the S3 buckets they are managing.

**Requirements:**
- The S3 bucket name is `nautilus-s3-391686905`.
- Enable **versioning** for this bucket.
- Configure the resources only in the **`us-east-1`** region.

---

## ✅ Solution

You can accomplish this task using either the AWS Management Console or the AWS CLI.

### Option 1: Using the AWS Management Console (GUI)

1. Open the provided Console URL and log in with your lab credentials.
2. In the top search bar, search for **S3** and open the S3 Dashboard.
3. On the left navigation pane (or main screen), click on **Buckets**.
4. In the list of buckets, find and click on the bucket named **`nautilus-s3-391686905`**.
5. Once inside the bucket's overview, click on the **Properties** tab near the top.
6. Look for the **Bucket Versioning** section (usually right at the top) and click the **Edit** button.
7. Change the toggle/selection to **Enable**.
8. Click **Save changes** at the bottom of the page.

### Option 2: Using the AWS CLI (Terminal)

If you prefer using the terminal on the AWS client machine, you can run a single command to enable versioning for the bucket:

```bash
aws s3api put-bucket-versioning \
  --bucket nautilus-s3-391686905 \
  --versioning-configuration Status=Enabled \
  --region us-east-1
```

*Note: The `s3api` is used here instead of the standard `s3` command because bucket-level configurations like versioning are managed via the API interface.*

---

### 🔍 Verification

To verify that versioning has been successfully enabled, you can run the following command:

```bash
aws s3api get-bucket-versioning \
  --bucket nautilus-s3-391686905 \
  --region us-east-1
```

You should see an output similar to this:

```json
{
    "Status": "Enabled"
}
```

Once you see this, you can safely click **Check** in the lab to complete the task!
