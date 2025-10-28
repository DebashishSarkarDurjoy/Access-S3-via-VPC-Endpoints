# Access-S3-via-VPC-Endpoints
AWS Architecture to access S3 using VPC Endpoints

# Solution design:
<img width="2150" height="1258" alt="image" src="https://github.com/user-attachments/assets/8df1fb08-9ffe-4a53-a68f-126d9ca42b2b" />

## Setup the foundational architecture:
<img width="2154" height="1242" alt="image" src="https://github.com/user-attachments/assets/81aeb40d-96d8-46a7-baab-c99de9e7913d" />

Steps:
  1. Go to VPC Console.
  2. Select Your VPCs and then Create VPC.
  3. Select VPC and More.
  4. Give a name for the project: "demo-project".
  5. Use the image below to make the selections:
     
       <img width="1920" height="1758" alt="screencapture-ap-southeast-2-console-aws-amazon-vpcconsole-home-2025-10-28-11_09_47" src="https://github.com/user-attachments/assets/11ff602c-8382-4a55-9573-6781fd744c1f" />
       
  6. Then click Create VPC.
  7. You will be able to see the following resource map, under Resource Map tab:

     <img width="1636" height="322" alt="Screen Shot 2025-10-28 at 11 12 59 am" src="https://github.com/user-attachments/assets/9ff1d7eb-9687-4807-a85d-55c2535046f1" />


## Launch an EC2 instance in the VPC:

Steps:
  1. Open EC2 console.
  2. Select Instances on the left hand navigation bar.
  3. Click Launch an Instance.
  4. Use the image below to setup the instance:
 
  <img width="1920" height="3026" alt="screencapture-ap-southeast-2-console-aws-amazon-ec2-home-2025-10-28-11_17_09" src="https://github.com/user-attachments/assets/d77bec6e-8868-47a9-a650-0a3903e59ca6" />

  5. Click Launch Instance.
  6. Use Instance Connect to access the instance.
  7. Configure the instance using this command: aws configure
  8. Create and use the access key ID, secret key, region name and default output format for the configuration.
  9. Now check to see if it can list the buckets in S3: aws s3 ls
  
  <img width="525" height="140" alt="Screen Shot 2025-10-28 at 11 22 42 am" src="https://github.com/user-attachments/assets/ab8e07ec-5d15-4bc1-9835-2d0cc869ef1b" />


## Create a VPC Endpoint

Steps:
  1. Go to VPC console.
  2. Select Endpoints from the left hand navigation bar.
  3. Create Endpoint.
  4. Use the image below to configure the Endpoint:
   
  <img width="1920" height="2369" alt="screencapture-ap-southeast-2-console-aws-amazon-vpcconsole-home-2025-10-28-11_26_18" src="https://github.com/user-attachments/assets/ef83e199-cb0a-4dc2-a1ba-78bbd4514784" />

  5. Click on create endpoint.
  6. Go to Manage route tables under Route Tables tab.
  7. Select the demo-project-rtb-public route table and click Modify route tables.

  <img width="1751" height="455" alt="Screen Shot 2025-10-28 at 11 47 44 am" src="https://github.com/user-attachments/assets/dac3d471-1680-46c8-b232-8e52bceb59d9" />

  9. Copy the endpoint ID.


## Create an S3 bucket:

Steps:
  1. Go to S3 console.
  2. Click on create bucket.
  3. Use the image below to configure the bucket:

     <img width="1920" height="2178" alt="screencapture-ap-southeast-2-console-aws-amazon-s3-bucket-create-2025-10-28-11_29_48" src="https://github.com/user-attachments/assets/142112cb-8da1-451c-a5f6-829689d1c787" />

  4. Click Create Bucket.
  5. Load some objects into the bucket.

     <img width="1777" height="430" alt="Screen Shot 2025-10-28 at 11 32 45 am" src="https://github.com/user-attachments/assets/9aba3acd-8c45-436d-8157-155538b30973" />

  6. Update the policy of the demo-project-bucket-debashish to only allow access from the VPC Endpoint ID:

  <img width="697" height="557" alt="Screen Shot 2025-10-28 at 11 44 14 am" src="https://github.com/user-attachments/assets/5492cfa9-6ba5-4d2e-b8d6-da9d53e46969" />

  7. Click Save Changes.
  8. This policy should now block all access, including the management console.


## Check connectivity using the EC2 instance:

Steps:
  1. List the objects in the bucket: aws s3 ls s3://demo-project-bucket-debashish

     <img width="676" height="81" alt="Screen Shot 2025-10-28 at 11 49 55 am" src="https://github.com/user-attachments/assets/a4a024f6-d4bc-4f34-a92e-a6015b5487cd" />

Note: The EC2 instance is able to access the bucket via the VPC Endpoint. Successful connectivity.

Now, create a text file within the EC2 instance and then copy it into the S3 bucket.

  2. Create a text file within EC2 instance: touch /tmp/test.txt
  3. Copy the text file into the bucket: aws s3 cp /tmp/test.txt s3://demo-project-bucket-debashish
  4. List the objects in the bucket: aws s3 ls s3://demo-project-bucket-debashish

     <img width="693" height="84" alt="Screen Shot 2025-10-28 at 11 55 49 am" src="https://github.com/user-attachments/assets/c8974412-18c7-4a80-b529-19cd8b32fcde" />

Note: However, we cannot see the objects from the S3 management console as the policy blocks it.

  <img width="1700" height="376" alt="Screen Shot 2025-10-28 at 11 57 47 am" src="https://github.com/user-attachments/assets/4bf2e4fa-4b11-432f-ab7a-c97a5a552bca" />



## Cleanup the resources:
- Delete the S3 bucket from via the EC2 instance:
    1. Delete all objects in the bucket: aws s3 rm s3://demo-project-bucket-debashish --recursive
    2. Delete the bucket: aws s3 rb s3://demo-project-bucket-debashish
       
  <img width="805" height="118" alt="Screen Shot 2025-10-28 at 12 00 27 pm" src="https://github.com/user-attachments/assets/3392dae2-ae31-42e0-b3d9-6361c59eacfc" />

- Terminate the EC2 instance
- Delete any access credentials.
- Delete the VPC.


# Project Completed





     
