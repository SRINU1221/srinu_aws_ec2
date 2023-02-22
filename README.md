# srinu_aws_ec2
This repository contains Python code for creating a VPC and subnets, deploying an EC2 instance, attaching it to a subnet, creating an S3 bucket, and deploying a static website using Python. It also includes code for installing Nginx on the EC2 instance using remote execution.
Prerequisites:

-->Python 3.7 or higher
-->Boto3 library for Python
-->Paramiko library for Python
-->AWS CLI installed and configured with appropriate credentials

Clone this repository to your local machine:https://github.com/SRINU1221/srinu_aws_ec2

Install the required Python libraries:
pip install boto3 paramiko
pip install json

Create the VPC and subnets by running the create_vpc_and_subnets.py script:
# Configure AWS credentials and region
os.environ['AWS_ACCESS_KEY_ID'] = 'YOUR_AWS_ACCESS_KEY_ID'
os.environ['AWS_SECRET_ACCESS_KEY'] = 'YOUR_AWS_SECRET_ACCESS_KEY'
os.environ['AWS_DEFAULT_REGION'] = 'YOUR_AWS_DEFAULT_REGION'

by this we can create vpc and subnet id's and This script will create a VPC and two subnets in the us-east-1 region. You can modify the script to create the VPC and subnets in a different region if desired.


-->Deploy the EC2 instance and attach it to a subnet by running the :
for creation of ec2 we need key_pair and security_froup id's
..>for key_pair:
        ec2.create_key_pair(KeyName="kgptalie")
--->next create a security group:
        res=ec2.create_security_group(GroupName="kgptalie",Description="kgptalie sg", VpcId= 'vpc-0cd5e44b50adbbb96')
        ec2.authorize_security_group_ingress(GroupId=gid,IpPermissions=[{'IpProtocol':'tcp','FromPort':80,'ToPort':80,'IpRanges':[{'CidrIp':'0.0.0.0/0'}]},{'IpProtocol':'tcp','FromPort':22,'ToPort':22,'IpRanges':[{'CidrIp':'0.0.0.0/0'}]}])
        
  code for creating a instance:
  ------------------------------
        ec2_resource=boto3.resource('ec2',region_name='ap-south-1',aws_access_key_id='AKIASF7Q3PXRRQU57SS7',aws_secret_access_key='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')
        ec2 = boto3.resource('ec2',region_name='ap-south-1',aws_access_key_id='AKIASF7Q3PXRRQU57SS7',aws_secret_access_key='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')
        vpc = ec2.create_vpc(CidrBlock='10.0.0.0/16')
        subnet = vpc.create_subnet(CidrBlock='10.0.1.0/24')
        instance = ec2.create_instances(ImageId='ami-0e742cca61fb65051', InstanceType='t2.micro', KeyName='kgptalie', SubnetId=subnet.id, MaxCount=1, MinCount=1)
        instance[0].wait_until_running()
        instance[0].reload()
        
        This script will launch a new t2.micro EC2 instance in the first subnet that was created. You can modify the script to use a different instance type or subnet if desired.
        
        
        
---->Create the S3 bucket and deploy a static website using the deploy_static_website:
-----------------------------------------------------------------------------------------

urlfor static website:  http://chauhanweb.s3-website.ap-south-1.amazonaws.com



steps:
1.Create an S3 client object by passing your AWS access key ID and secret access key to the boto3.client() method and Replace 'ACCESS_KEY' and 'SECRET_KEY' with your actual AWS access key ID and secret access key.
2.Define a function called create_bucket() that will create an S3 bucket. The function takes two arguments: bucket_name and region.This function uses the create_bucket() method of the S3 client object to create a new bucket with the specified name and region. If the region is not specified, the bucket will be created in the default region (us-east-1).
3.Define a function called deploy_website() that will upload the static website files to the S3 bucket and configure it as a static website. The function takes three arguments: bucket_name, website_dir, and index_doc.This function uses the upload_file() method of the S3 client object to upload the index_doc file from the website_dir directory to the S3 bucket. It then uses the put_bucket_website() method to configure the bucket as a static website and specify the index_doc file as the default index document.
4.Call the create_bucket() and deploy_website() functions with the appropriate arguments to create the S3 bucket and deploy the static website.
5.This code will create an S3 bucket called my-static-website-bucket in the us-west-2 region, upload the index.html file from the website directory to the bucket, and configure it as a static website with the index.html file as the default index document.

Make sure to replace the bucket name, region, website directory, and index document with your own values.



-->Install Nginx on the EC2 instance using remote execution:
-------------------------------------------------------------------


To install Nginx on an EC2 instance using remote execution, you can use the paramiko library in Python to connect to the instance via SSH and run the necessary commands to install and start Nginx.


Install Nginx by running the command sudo amazon-linux-extras install nginx1.12.
Start the Nginx service by running the command sudo service nginx start.
Make sure to replace the hostname, username, and key_filename variables with the appropriate values for your EC2 instance. Also, make sure that your security group allows inbound traffic on port 22 (SSH) from your IP address, so that you can connect to the instance via SSH.








 
        
