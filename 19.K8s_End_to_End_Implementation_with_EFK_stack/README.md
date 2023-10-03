# Fleet Management System Setup Guide

**WARNING:** You **MUST** delete your cluster when finished.

## Step 1: Install eksctl

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

## Step 2: Update AWS CLI to Version 2

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Now, log out of your shell and back in again.

## Step 3: Set Up a Group

Set up a group with the following permissions:

- AmazonEC2FullAccess
- IAMFullAccess
- AWSCloudFormationFullAccess

You also need to create an inline policy with the following JSON:

```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "eks:*",
      "Resource": "*"
    },
    {
      "Action": [
        "ssm:GetParameter",
        "ssm:GetParameters"
      ],
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
```

## Step 4: Add a User to the Group

Use the AWS Management Console to add a user to your new group and then use aws configure to input the credentials.

## Step 5: Install kubectl

Warning: Check the current default Kubernetes version supplied with EKS and install a matching version of kubectl.

```bash
export RELEASE=<enter default EKS version number here, e.g., 1.17.0>
curl -LO https://storage.googleapis.com/kubernetes-release/release/v$RELEASE/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

Check the version with kubectl version --client.

## Step 6: Start Your Cluster!

```bash
eksctl create cluster --name fleetman --nodes-min=3
```

## Step 7: Create an Inline Policy for AmazonElasticBlockStoreFullAccess

```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:CreateVolume",
        "ec2:DeleteVolume",
        "ec2:DescribeVolumes",
        "ec2:AttachVolume",
        "ec2:DetachVolume",
        "ec2:CreateSnapshot",
        "ec2:DeleteSnapshot",
        "ec2:DescribeSnapshots",
        "ec2:CreateTags"
      ],
      "Resource": "*"
    }
  ]
}
```

## Step 8: For StorageClass, Install CSI Driver

```bash
helm uninstall aws-ebs-csi-driver -n kube-system
helm install aws-ebs-csi-driver aws-ebs-csi-driver/aws-ebs-csi-driver --namespace kube-system
```
