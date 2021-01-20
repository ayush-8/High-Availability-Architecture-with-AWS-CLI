# High-Availability-Architecture-with-AWS-CLI

First of all to create this particular architecture we need to have an instance running in AWS Console.
So, launching an instance in AWS by this simple command

`aws ec2 run-instances [option]...`
![](Images/Screenshot%20(100).png)

Here, you can see the status is Pending.

But after sometime it launches and is running. Take note of the IP of the instance to connect via SSH and do the further configurations.
![](Images/Screenshot%202021-01-20%20155723.png)

- Webserver configured on EC2 Instance
Connect to instance using putty and proper authorization key.
Installing Apache web server and php on instance.

`yum install httpd php -y`
![](Images/Screenshot%202021-01-20%20160956.png)

Starting the service and enabling it for permanent start.

`systemctl enable httpd --now`
![](Images/Screenshot%202021-01-20%20183719.png)

- Document Root(/var/www/html) made 
persistent by mounting on EBS Block Device.

For this first lets create an EBS Volume.

`aws ec2 create-volume`
![](Images/Screenshot%202021-01-20%20161516.png)

Then watch the status of volume by-

`aws ec2 describe-instances`
![](Images/Screenshot%202021-01-20%20161840.png)
![](Images/Screenshot%202021-01-20%20163348.png)

After the volume is attached, a partition needs to created to store data on it or use it.
Looking the devices/volumes attached

`fdisk -l`

Seeing the mounted volumes

`df -h`
![](Images/Screenshot%202021-01-20%20163640.png)

Creating a partition
![](Images/Screenshot%202021-01-20%20163825.png)

Formatting and Mounting it to the Document Root Directory

`/var/www/html/`

by using:
`mkfs /dev/sdh`

`mount /dev/sdh /var/www/html`
![](Images/Screenshot%202021-01-20%20163936.png)

- Static objects used in code such as 
pictures stored in S3

Now, I am taking a picture of the popular anime "Your Name" and upload it to AWS S3 bucket.

First let's create an S3 bucket.

`aws s3api create-bucket`
![](Images/Screenshot%202021-01-20%20165333.png)
Altering it's permissions and giving it public access.

![](Images/Screenshot%202021-01-20%20173120.png)
Uploading the image file to S3 bucket.

![](Images/Screenshot%202021-01-20%20193045.png)

- Setting up Content Delivery Network using
CloudFront and using the origin domain as S3 bucket.

Create a CloudFront Distributtion. We'll Use WebUI for this.

Choose S3 bucket in Origin Domain.
![](Images/Screenshot%20(104).png)

Take note of the Domain name once the distribution is deployed. (This is the URL of CloudFront that will redirect to the S3 Bucket)
![](Images/Screenshot%20(102).png)

- Finally place the Cloud Front URL on the
webapp code for security and low latency.

Let's create a simple and sober html file for showing-off this code.
![](Images/Screenshot%202021-01-20%20183641.png)

Just put the cloudfront url in the image source.
![](Images/Screenshot%202021-01-20%20192547.png)
Okay. It was a single line.

Now, Save it and test it.

Take the instance IP.
![](Images/Screenshot%202021-01-20%20192639.png)

ANd BANG!
![](Images/Screenshot%202021-01-20%20192657.png)
