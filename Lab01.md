s3://formation-aws-fondamentaux/Formation onepoint Developing on AWS - Public0.pdf 
# AWS-FUNDAMENTALS-EXERCISES

## **EXERCISE 1.1**

### **Task 1 : Launch and Connect to a Linux Instance**
In this exercise, you will launch a new Linux instance, log in with SSH, and install any security updates. 
1. Launch an instance in the Amazon EC2 console. 
2. Choose the Amazon Linux AMI (eg:Amazon Linux 2 AMI (HVM)). 
3. Choose the t2.micro instance type. 
4. Launch the instance in the default VPC. 
5. Assign the instance a public IP address. 
6. In the Advanced Details section, enter the following text as UserData : 

              ```
              #!/bin/sh
              yum -y install httpd
              chkconfig httpd on
              /etc/init.d/httpd start
              ```

7. Add a tag to the instance of Key: Name, Value: Exercise 1.1.
8. Create a new security group called FistName-internet-firewall-sg. 
9. Add a rule to FistName-internet-firewall-sg allowing SSH access from the IP address of your workstation (www.WhatsMyIP.org is a good way to determine your IP address). 
10. Launch the instance. 
11. When prompted for a key pair, choose a key pair you already have or create a new one and download the private portion. Amazon generates a keyname.pem file, and you will need a keyname.ppk file to connect to the instance via SSH. Puttygen.exe is one utility that will create a .ppk file from a .pem file. 
12. SSH into the instance using the public IP address, the user name ec2-user, and the keyname.ppk file. 
13. From the command-line prompt, run sudo yum update—security -y. 

### **Task 2 : Monitor your Instance**
14. Click in the monotoring tab
15. In the action menu > select instance settings > Get system Log
16. Scroll through the output and note that the http package was installed from the user data that your added when you created the instance

### **Task 3 : Access Metadata**
17. At the Linux command prompt, retrieve a list of the available metadata by typing: curl http://169.254.169.254/latest/meta-data/ 
18. To see a value, add the name to the end of the URL. For example, to see the security groups, type: curl http://169.254.169.254/latest/meta-data/security-groups 
19. Try other values as well. Names that end with a / indicate a longer list of sub-values. 
20. Close the SSH window and terminate the instance

## **EXERCISE 1.2**

### **Task 1 : Launch a Windows Instance with Bootstrapping**
In this exercise, you will launch a Windows instance and specify a very simple bootstrap script.
You will then confirm that the bootstrap script was executed on the instance. 

1. Launch an instance in the Amazon EC2 console.
2. Choose the Microsoft Windows Server 2019 Base AMI. 
3. Choose the m3.medium instance type.
4. Launch the instance in the default VPC. 
5. Assign the instance a public IP address. 
6. In the Advanced Details section, enter the following text as UserData: 

        ```
        <script> md c:\temp </script> 
        <powershell>
           Import-Module ServerManager
           Install-WindowsFeature web-server, web-webserver
           Install-WindowsFeature web-mgmt-tools
        </powershell>
        ```

7. Add a tag to the instance of Key: Name, Value: Exercise 1.2. 
8. Use the FistName-internet-firewall-sg security group from Exercise 1.1. 
9. Launch the instance. 
10. Use the key pair from Exercise 1.1. 
11. On the Connect Instance UI, decrypt the administrator password and then download the RDP file to attempt to connect to the instance. Your attempt should fail because the FistName-internet-firewall-sg security group does not allow RDP access. 
12. Open the FistName-internet-firewall-sg security group and add a rule that allows RDP access from your IP address. 
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
28. In the action menu > select instance state > click on Terminate
29. End the RDP session and terminate the instance.

## **EXERCISE 1.3 Launch a Spot Instance**

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



## **EXERCISE 2**

### **Task 1 : Create an Amazon Simple Storage Service (Amazon S3) Bucket**
In this exercise, you will create a new Amazon S3 bucket in your selected region. You will use this bucket in the following exercises.

1. Log in to the AWS Management Console. 
2. Choose an appropriate region, such as US West (Oregon). 
3. Navigate to the Amazon S3 console. Notice that the region indicator now says Global. Remember that Amazon S3 buckets form a global namespace, even though each bucket is created in a specific region. 
4. Start the create bucket process. 
5. When prompted for Bucket Name, use mynewbucket. 
6. Choose a region, such as US West (Oregon). 
7. Try to create the bucket. You almost surely will get a message that the requested bucket name is not available. Remember that a bucket name must be unique globally. 
8. Try again using your surname followed by a hyphen and then today’s date in a sixdigit format as the bucket name (a bucket name that is not likely to exist already).
You should now have a new Amazon S3 bucket.

### **Task 2 : Upload, Make Public, Rename, and Delete Objects in Your Bucket**
In this exercise, you will upload a new object to your bucket. You will then make this object public and view the object in your browser. You will then rename the object and finally delete it from the bucket.

#### **Upload an Object**
1. Load your new bucket in the Amazon S3 console.
2. Select Upload, then Add Files. 
3. Locate a file on your PC that you are okay with uploading to Amazon S3 and making public to the Internet. (We suggest using a non-personal image file for the purposes of this exercise.) 
4. Select a suitable file, then Start Upload. You will see the status of your file in the Transfers section. 
5. After your file is uploaded, the status should change to Done. The file you uploaded is now stored as an Amazon S3 object and should be now listed in the contents of your bucket.

#### **Open the Amazon S3 URL**
6. Now open the properties for the object. The properties should include bucket, name, and link. 
7. Copy the Amazon S3 URL for the object. 
8. Paste the URL in the address bar of a new browser window or tab. You should get a message with an XML error code AccessDenied. Even though the object has a URL, it is private by default, so it cannot be accessed by a web browser.

#### **Make the Object Public**
9. Go back to the Amazon S3 Console and select Make Public. (Equivalently, you can change the object’s permissions and add grantee Everyone and permissions Open/Download.)
10. Copy the Amazon S3 URL again and try to open it in a browser or tab. Your public image file should now display in the browser or browser tab. 

#### **Rename Object**
11. In the Amazon S3 console, select Rename. 
12. Rename the object, but keep the same file extension. 
13. Copy the new Amazon S3 URL and try to open it in a browser or tab. You should see the same image file. Delete the Object 
14. In the Amazon S3 console, select Delete. Select OK when prompted if you want to delete the object. 
15. The object has now been deleted. 
16. To verify, try to reload the deleted object’s Amazon S3 URL. You should once again get the XML AccessDenied error message.

### **Task 3 : Enable Version Control**

In this exercise, you will enable version control on your newly created bucket. 

#### **Enable Versioning**
1. In the Amazon S3 console, load the properties of your bucket. Don’t open the bucket. 
2. Enable versioning in the properties and select OK to verify. Your bucket now has versioning enabled. (Note that versioning can be suspended, but not turned off.) 

#### **Create Multiple Versions of an Object**
3. Create a text file named foo.txt on your computer and write the word blue in the text file. 
4. Save the text file to a location of your choosing.
5. Upload the text file to your bucket. This will be version 1. 
6. After you have uploaded the text file to your bucket, open the copy on your local computer and change the word blue to red. Save the text file with the original filename. 
7. Upload the modified file to your bucket. 
8. Select Show Versions on the uploaded object.

You will now see two different versions of the object with different Version IDs and possibly different sizes. Note that when you select Show Version, the Amazon S3 URL now includes the version ID in the query string after the object name.

### **Task 3 : Delete an Object and Then Restore It**
In this exercise, you will delete an object in your Amazon S3 bucket and then restore it. 

#### **Delete an Object**
1. Open the bucket containing the text file for which you now have two versions. 
2. Select Hide Versions. 
3. Select Delete, and then select OK to verify. 
4. Your object will now be deleted, and you can no longer see the object. 
5. Select Show Versions. Both versions of the object now show their version IDs.

#### **Restore an Object**  
6. Open your bucket. 
7. Select Show Versions. 
8. Select the oldest version and download the object. Note that the filename is simply foo.txt with no version indicator. 
9. Upload foo.txt to the same bucket. 
10. Select Hide Versions, and the file foo.txt should re-appear.

### **Task 4 : Enable Static Hosting on Your Bucket**
In this exercise, you will enable static hosting on your newly created bucket.

1. Select your bucket in the Amazon S3 console. 
2. In the Properties section, select Enable Website Hosting. 
3. For the index document name, enter **index.txt**, and for the error document name, enter **error.txt.** 
4. Use a text editor to create two text files and save them as **index.txt** and **error.txt**. In the index.txt file, write the phrase “**Hello World**,” and in the error.txt file, write the phrase “**Error Page.**” Save both text files and upload them to your bucket. 
5. Make the two objects public. 
6. Copy the Endpoint: link under Static Website Hosting and paste it in a browser window or tab. You should now see the phrase "Hello World" displayed. 
7. In the address bar in your browser, try adding a forward slash followed by a madeup filename (for example, /test.html). You should now see the phrase "Error Page" displayed. 
8. To clean up, delete all of the objects in your bucket and then delete the bucket itself.
