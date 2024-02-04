# Create Multiple IAM Users using AWS CLI

This repository provides a simple script to create multiple IAM (Identity and Access Management) users on AWS using the AWS CLI. Follow the steps below to set up and use the script.

## Step 1: Connect to EC2 Linux instance using SSH

Connect to your EC2 Linux instance using SSH. If you don't have an EC2 instance yet, create one in the AWS Management Console.

```bash
ssh -i <your-key.pem> ec2-user@<your-ec2-instance-ip>
```

Replace `<your-key.pem>` with your SSH key and `<your-ec2-instance-ip>` with your EC2 instance's public IP address.

## Step 2: Create a UserList.txt File

Create a text file named `UserList.txt` with a list of usernames for which you want to create AWS IAM accounts. Use a text editor like `nano` to create the file.

```bash
nano UserList.txt
```

Type each username on a new line and save the file.

## Step 3: Configure AWS CLI

Run the following command to configure AWS CLI with your root user credentials.

```bash
aws configure
```

Provide your access key, secret key, default region, and output format as prompted.

## Step 4: Create a Shell Script

Create a shell script file named `users.sh` using the following command:

```bash
nano users.sh
```

Add the following script to `users.sh`:

```bash
#!/bin/bash

while read user
do
    password="abcd1234"

    aws iam create-user --user-name "$user"

    aws iam attach-user-policy --user-name "$user" --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

    aws iam create-login-profile --user-name "$user" --password "$password" --password-reset-required

    echo "created $user with password $password"
done < UserList.txt
```

Save the file.

## Step 5: Execute the Script

Execute the following command to create users listed in `UserList.txt` file.

```bash
bash users.sh
```

This script will create IAM users, attach the AdministratorAccess policy, and set a default password for each user.

Feel free to customize the script, adjust permissions, and ensure AWS CLI is properly configured before running the script.
