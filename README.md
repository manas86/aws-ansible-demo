#### Step by step guide to run Ansible in AWS EC2 Instances

##### Prerequisites
An AWS EC2 instance

##### Let's Start

1.  Login to your AWS EC2 instance .
2.  Click "Launch Instances", 
    Now, We will go thru step by step to create multiple instances of Redhat.
    For this demo, I keep it as 2 instances. one for Ansible-Control-Server and WebServer.
    PS: After each step remember to click Next: Button
    
    ![Step1](/Images/Step1.png)
    ![Step2](/Images/Step2.png) 
    ![Step3](/Images/Step3.png) 
    > This step allows you to create multiple instances
    ![Step4](/Images/Step4.png)
    ![Step5](/Images/Step5.png)
    > Next enable the service group with ssh port 22. Since Ansible is ssh connection only.
    ![Step6](/Images/Step6.png)
    ![Step7](/Images/Step7.png)
    > Before "Launch" make sure you select a new key-pair. And copy the keyparname.pem into you local .ssh folder.
    ![Step8](/Images/Step8.png)
    ![Copy AWS Key to your Local](/Images/Copy%20AWS%20key%20to%20Local.png)
     