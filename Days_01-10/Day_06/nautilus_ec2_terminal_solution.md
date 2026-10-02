# Nautilus AWS EC2 Task — Terminal Solution

## Requirements

- Instance name: `devops-ec2`
- AMI: Amazon Linux
- Instance type: `t2.micro`
- RSA key pair: `devops-kp`
- Security group: default security group
- Region: `us-east-1`

## 1. Verify AWS Credentials

Run on the `aws-client` host:

```bash
aws sts get-caller-identity
```

## 2. Set Region

```bash
export AWS_DEFAULT_REGION=us-east-1
```

Verify:

```bash
echo $AWS_DEFAULT_REGION
```

Expected:

```text
us-east-1
```

## 3. Find the Default VPC

```bash
VPC_ID=$(aws ec2 describe-vpcs   --filters Name=is-default,Values=true   --query 'Vpcs[0].VpcId'   --output text)

echo $VPC_ID
```

## 4. Find the Default Security Group

```bash
SG_ID=$(aws ec2 describe-security-groups   --filters Name=vpc-id,Values=$VPC_ID Name=group-name,Values=default   --query 'SecurityGroups[0].GroupId'   --output text)

echo $SG_ID
```

## 5. Create the RSA Key Pair

```bash
aws ec2 create-key-pair   --key-name devops-kp   --key-type rsa   --query 'KeyMaterial'   --output text > devops-kp.pem
```

Verify:

```bash
aws ec2 describe-key-pairs --key-names devops-kp
```

## 6. Secure the Private Key

```bash
chmod 400 devops-kp.pem
```

## 7. Get an Amazon Linux AMI

```bash
AMI_ID=$(aws ssm get-parameter   --name /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64   --query 'Parameter.Value'   --output text)

echo $AMI_ID
```

## 8. Launch the EC2 Instance

```bash
aws ec2 run-instances   --image-id "$AMI_ID"   --instance-type t2.micro   --key-name devops-kp   --security-group-ids "$SG_ID"   --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]'
```

## 9. Get the Instance ID

```bash
INSTANCE_ID=$(aws ec2 describe-instances   --filters Name=tag:Name,Values=devops-ec2             Name=instance-state-name,Values=pending,running   --query 'Reservations[0].Instances[0].InstanceId'   --output text)

echo $INSTANCE_ID
```

## 10. Verify the Instance

```bash
aws ec2 describe-instances   --instance-ids "$INSTANCE_ID"   --query 'Reservations[0].Instances[0].{Name:Tags[?Key==`Name`]|[0].Value,State:State.Name,Type:InstanceType,Key:KeyName,AMI:ImageId,SecurityGroup:SecurityGroups[0].GroupId}'   --output table
```

Expected configuration:

```text
Name            devops-ec2
State           running
Type            t2.micro
Key             devops-kp
Security Group  default security group
AMI             Amazon Linux
Region          us-east-1
```

## Complete Copy-Paste Sequence

```bash
export AWS_DEFAULT_REGION=us-east-1

aws sts get-caller-identity

VPC_ID=$(aws ec2 describe-vpcs   --filters Name=is-default,Values=true   --query 'Vpcs[0].VpcId'   --output text)

echo "Default VPC: $VPC_ID"

SG_ID=$(aws ec2 describe-security-groups   --filters Name=vpc-id,Values=$VPC_ID Name=group-name,Values=default   --query 'SecurityGroups[0].GroupId'   --output text)

echo "Default Security Group: $SG_ID"

aws ec2 create-key-pair   --key-name devops-kp   --key-type rsa   --query 'KeyMaterial'   --output text > devops-kp.pem

chmod 400 devops-kp.pem

AMI_ID=$(aws ssm get-parameter   --name /aws/service/ami-amazon-linux-latest/al2023-ami-kernel-default-x86_64   --query 'Parameter.Value'   --output text)

echo "Amazon Linux AMI: $AMI_ID"

aws ec2 run-instances   --image-id "$AMI_ID"   --instance-type t2.micro   --key-name devops-kp   --security-group-ids "$SG_ID"   --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=devops-ec2}]'
```

Then verify:

```bash
aws ec2 describe-instances   --filters Name=tag:Name,Values=devops-ec2   --query 'Reservations[].Instances[].{ID:InstanceId,Name:Tags[?Key==`Name`]|[0].Value,State:State.Name,Type:InstanceType,Key:KeyName}'   --output table
```

Once the instance reaches `running`, click **Check** in the lab.

> **Important:** Do not share the downloaded `devops-kp.pem` private key.
