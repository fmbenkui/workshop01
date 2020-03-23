# AWS-FUNDAMENTALS-WorkShop01

## **Lab01 : Explore Amazon Linux Ec2 Instance**

### **Task 1 : Launch and Connect to a Linux Instance**
In this exercise, you will launch a new Linux instance, log in with SSH, and install any security updates. 
1. Launch an instance in the Amazon EC2 console. 
2. Choose the Amazon Linux AMI (eg:Amazon Linux 2 AMI (HVM)). 
3. Choose the **t2.micro** instance type. 
4. Launch the instance in the default VPC. 
5. Assign the instance a public IP address. 
6. In the Advanced Details section, enter the following text as UserData : 

              ```
              #!/bin/bash
              #Install Apache Web Server and PHP
              yum install -y httpd mysql 
              amazon-linux-extras install -y php7.2
              #Download Lab files
              wget https://github.com/fmbenkui/workshop01/raw/master/ec2-info.zip
              unzip ec2-info -d /var/www/html/
              echo "<?php phpinfo(); ?>" > /var/www/html/info.php
              #Turn on web server
              chkconfig httpd on
              service httpd start
              ```

7. Add a tag to the instance of Key: Name, Value: «fistName-web-linux» .
8. Create a new security group called «fistName-internet-firewall-sg». 
9. Add a rule to «FistName-internet-firewall-sg» allowing SSH access from the IP address of your workstation (www.WhatsMyIP.org is a good way to determine your IP address). 
10. Launch the instance. 
11. When prompted for a key pair, choose a key pair you already have or create a new one and download the private portion.           Amazon generates a fistName-key.pem file that you can use Downlad mobaxterm (https://mobaxterm.mobatek.net/download.html) to connect to the instance via SSH. If you already have putty you will need a keyname.ppk file to connect to the instance via SSH. Puttygen.exe is one utility that will create a .ppk file from a .pem file. 
12. SSH into the instance using the public IP address, the user name ec2-user, and the keyname.ppk file. 
13. From the command-line prompt, run **sudo yum update --security -y**. 

### **Task 2 : Monitor your Instance**
14. Click in the monotoring tab
15. In the action menu > select instance settings > Get system Log
16. Scroll through the output and note that the http package was installed from the user data that your added when you created the instance

### **Task 3 : Access Metadata**
17. At the Linux command prompt, retrieve a list of the available metadata by typing: **curl http://169.254.169.254/latest/meta-data/** 
18. To see a value, add the name to the end of the URL. For example, to see the security groups, type: **curl http://169.254.169.254/latest/meta-data/security-groups**
19. Try other values as well. Names that end with a / indicate a longer list of sub-values. 
20. Close the SSH window and terminate the instance

## **Lab02 : Explore Amazon Windows Ec2 Instance**

### **Task 1 : Launch a Windows Instance with Bootstrapping**
In this exercise, you will launch a Windows instance and specify a very simple bootstrap script.
You will then confirm that the bootstrap script was executed on the instance. 

1. Launch an instance in the Amazon EC2 console.
2. Choose the Microsoft Windows Server 2019 Base AMI. 
3. Choose the**m3.medium** instance type.
4. Launch the instance in the default VPC. 
5. Assign the instance a public IP address. 
6. In the Advanced Details section, enter the following text as UserData: 

        ```
        <powershell>
           Set-ExecutionPolicy Unrestricted -Force
           New-Item -ItemType directory -Path 'C:\temp'
           Import-Module ServerManager
           Install-WindowsFeature web-server, web-webserver
           Install-WindowsFeature web-mgmt-tools
        </powershell>
        ```

7. Add a tag to the instance of Key: Name, Value: «fistName-web-windows».
8. Use the «fistName-internet-firewall-sg» security group from Exercise 1.1.
9. Launch the instance. 
10. Use the key pair from Exercise 1.1. 
11. On the Connect Instance UI, decrypt the administrator password and then download the RDP file to attempt to connect to the instance. Your attempt should fail because the «fistName-internet-firewall-sg» security group does not allow RDP access. 
12. Open the «fistName-internet-firewall-sg» security group and add a rule that allows RDP access from your IP address. 
13. Attempt to access the instance via RDP again. 
14. Once the RDP session is connected, open Windows Explorer and confirm that the c:\temp folder has been created.

### **Task 2 : Confirm That Instance Stores Are Lost When an Instance Is Stopped**
15. Create a new folder named c:\test. 
16. Log out of the RDP session. 
17. In the console, set the state of the instance to Stopped. 
18. Once the instance is stopped, start it again. 
19. Log back into the instance using RDP. 
20. Open Windows Explorer and confirm that the c:\test folder is gone.

### **Task 3 : Resize your Instance**
 21. In the action menu > select instance state > click on Stop
 22. Wait for the Intance state to display stopped
 23. In the action menu > select instance settings > change instance type
 24. then configure instance type to t2.small
 25. click apply
 
### **Task 4 : Test Termination Protection**
26. In the action menu > select instance settings > change termination protection > set to be Enable.
27. In the console, set the state of the instance to Stopped.
28. In the action menu > select instance state > click on Terminate.
29. End the RDP session and terminate the instance.

## **Lab03 : Launch a Spot Instance**

In this exercise, you will create a Spot Instance.
1. In the Amazon EC2 console, go to the Spot Request page.
2. Look at the pricing history for m3.medium, especially the recent price. 
3. Make a note of the most recent price and Availability Zone.
4. Launch an instance in the Amazon EC2 console. 
5. Choose the Amazon Linux AMI. 
6. Choose the t2.medium instance type. 
7. On the Configure Instance page, request a Spot Instance. 
8. Launch the instance in the Default VPC.
10. Request a Spot Instance and enter a bid a few cents above the recorded Spot price. 
11. Finish launching the instance. 
12. Go back to the Spot Request page. Watch your request. If your bid was high enough, you should see it change to Active and an instance ID appear. 
13. Find the instance on the instances page of the Amazon EC2 console. Note the Lifecycle field in the Description that says Spot. 
14. Once the instance is running, terminate it.
