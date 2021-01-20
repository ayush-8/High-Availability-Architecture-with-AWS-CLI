# High-Availability-Architecture-with-AWS-CLI

First of all to create this particular architecture we need to have an instance running in AWS Console.
So, launching an instance in AWS by this simple command

`aws ec2 run-instances [option]...`
![](Images/Screenshot%20(100).png)

Here, you can see the status is Pending.

But after sometime it launches and is running. Take note of the IP of the instance to connect via SSH and do the further configurations.
![](Images/Screenshot%202021-01-20%20155723.png)

- Webserver configured on EC2 Instance

Installing Apache web server and php on instance.

`yum install httpd php -y`
![](Images/Screenshot%202021-01-20%20160956.png)

Starting the service and enabling it for permanent start.

- Document Root(/var/www/html) made 
persistent by mounting on EBS Block Device.
- Static objects used in code such as 
pictures stored in S3
- Setting up Content Delivery Network using
CloudFront and using the origin domain as S3 bucket.
- Finally place the Cloud Front URL on the
webapp code for security and low latency.
