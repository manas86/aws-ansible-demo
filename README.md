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
3.  Then on AWS dashbord, we will see 2 instances are running with same name.
    Change one instance name as "AnsibleServer" and "WebServer"   
    ![](/Images/Rename%20Instances.png)  
4.  Now connect to both instances using ssh connection as below
    ![](/Images/AWS%20HostName.png)
    After connecting via ssh to your own instances make sure you run `yum update`
    ![](/Images/SSH%20Connections.png) on both instances.
5.  Let's call one instance as master and other as target. 
    Now follow below steps:
    
    1.   On master: 
  
        * Add a EPEL (Extra Packages for Enterprise Linux)third party repository to get packages for Ansible
        `rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`
        * Install Ansible
        `yum install ansible -y `
        * Check Ansible version
        `ansible --version`
    2.  Now on both master and target:
        * Create a new user for ansible administration & grant admin access to user
        ```
        useradd ansadmin
        passwd ansadmin (remember the password)
        use visudo --> to add below lines
        ansadmin        ALL=(ALL)       NOPASSWD: ALL
        ```
        * Since this is at learning stage, I use password based authentication 
        `sed -ie 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config`
        * And then quick restart
        `service sshd restart`
        
    3.  Now login to target:
        * run `ifconfig` to check the IP address of target machine.
        ![](/Images/ip%20addr%20target%20machine.png)
        
    4.  Now login as `ansadmin` on Master node
        ![](/Images/ansadmin%20ssh%20login.png)
        * generate ssh key
        ` ssh-keygen`
        * Copy keys onto all ansible target nodes
        `ssh-copy-id <target-server>`
        ![](/Images/copy%20ssh%20key%20to%20target%20node.png)
        while coping this will ask for the password.
        * Now quick test to target ip will help a lot.
        `ssh target-ip`
        
    5.  Now on Master
        * Update target servers information on /etc/ansible/hosts file.
        ![](/Images/ansible%20host.png)
        
    6.  Now on `ansadmin` session of master node
        * Run ansible command as ansadmin user it should be successful 
        ` ansible all -m ping`
        ![](/Images/AnsibleCommand.png)
        
     7. Now run further simple commands:
        * `copy`
        ![ansadmin window](/Images/ansadmin-1.png)
        ![target window](/Images/target-1.png)
        * `httpd`
        ![failed](/Images/failed%20htpsd.png) 
        As yum can't be installed without root.
        ![success](/Images/success%20httpd.png)
        ![target instances](/Images/httpd%20on%20target.png)
        * Start httpd service on target system
        ![start httpd](/Images/start%20httpd%20master.png)
        ![target instance](/Images/httpd%20running%20on%20target.png)