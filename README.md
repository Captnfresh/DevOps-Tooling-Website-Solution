# DevOps-Tooling-Website-Solution

In a previous project, we implemented a WordPress-based solution that can be used as a fully-fledged website or blog. Moving forward, we want to introduce a set of DevOps tools that will help your team in day-to-day activities in managing, developing, testing, deploying, and monitoring different projects.

## Setup and Technologies Used in this Project

You will implement a tooling website solution that provides easy access to DevOps tools within the corporate infrastructure. The solution will include the following components:

1. Infrastructure: AWS
2. Webserver Linux: Red Hat Enterprise Linux 8
3. Database Server: Ubuntu 24.04 + MySQL
4. Storage Server: Red Hat Enterprise Linux 8 + NFS Server
5. Programming Language: PHP
6. Code Repository: GitHub


## Architecture

In this project, you will implement a tooling website solution that makes access to DevOps tools within the corporate infrastructure easily accessible.
The architecture consists of the following components:

1. Infrastructure: AWS Cloud
2. Web Server OS: Red Hat Enterprise Linux 8
3. Database Server OS: Ubuntu 24.04 + MySQL
4. Storage Server: Red Hat Enterprise Linux 8 + NFS Server
5. Programming Language: PHP
6. Code Repository: GitHub

The infrastructure will follow a three-tier architecture pattern with stateless Web Servers sharing a common database and using Network File System (NFS) as shared storage.

## Architecture Diagram

image 1

It is important to know what storage solution is suitable for what use cases, for this - we need to answer following questions: what data will be stored, in what format, how this data will be accessed, by whom, from where, how frequently, etc. Base on this you will be able to choose the right storage system for your solution.

## Step 1 -  Prepare NFS Server

1. Spin up a new EC2 instance with RHEL Linux 9 Operating System.
2. Based on your LVM experience from Project 6, Configure LVM on the Server. Create 3 volumes in the same AZ as your Web Server EC2.

   image 2

3. SSH into your instance and update your machine:

   ```
   sudo yum update -y
   ```
   image 3

   
5. List the disk:

   ```
   lsblk
   ```

6. Use df -h command to see all mounts and free space:

   ```
   df -h
   ```

7. Use gdisk utility to create a single partition on each of the 3 disks:

   ```
   sudo gdisk /dev/xvdf
   ```
   image 4


9. Install lvm2 package: creating logical volumes, which can be resized or moved without needing to unmount file systems.

    ```
    sudo yum install lvm2 -y
    ```

10. Check for available partitions.

    ```
    sudo lvmdiskscan
    ```
    image 5



11. Create Physical Volumes Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

    ```
    sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
    ```

12. Verify that your Physical volume has been created successfully

    ```
    sudo pvs
    ```

13. Use vgcreate utility to add all 3 PVs to a volume group (VG) Name the VG webdata-vg

    ```
    sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
    ```

14. Verify that your VG has been created successfully

    ```
    sudo vgs
    ```

15. Create Logical Volumes Use lvcreate utility to create logical volumes

    ```
    sudo lvcreate -L 14G -n lv-apps webdata-vg
    sudo lvcreate -L 14G -n lv-logs webdata-vg
    sudo lvcreate -L 14G -n lv-opt  webdata-vg
    ```

16. Verify that our Logical Volume has been created successfully

    ```
    sudo lvs
    ```

    image 6
        
17. Verify the entire setup #view complete setup - VG , PV, and LV

    ```
    sudo vgdisplay -v
    ```

18. Format the disks as xfs instead ext4 you will have to format them as xfs. Ensure there are 3 Logical Volumes lv-opt lv-apps, and lv-logs

    ```
    sudo lsblk
    ```

19. Format the Logical Volumes as XFS:

    ```
    sudo mkfs.xfs /dev/webdata-vg/lv-apps
    sudo mkfs.xfs /dev/webdata-vg/lv-logs
    sudo mkfs.xfs /dev/webdata-vg/lv-opt
    ```

20. Create mount points on /mnt directory for the logical volumes:

    ```
    sudo mkdir -p /mnt/apps /mnt/logs /mnt/opt
    ```

21. Mount Logical Volumes

    ```
    sudo mount /dev/webdata-vg/lv-apps /mnt/apps
    sudo mount /dev/webdata-vg/lv-logs /mnt/logs
    sudo mount /dev/webdata-vg/lv-opt /mnt/opt
    ```

22. Add Mount Points to /etc/fstab 

    ```
    sudo vi /etc/fstab
    ```

23. Add the following lines:

    ```
    /dev/webdata-vg/lv-apps /mnt/apps xfs defaults 0 0
    /dev/webdata-vg/lv-logs /mnt/logs xfs defaults 0 0
    /dev/webdata-vg/lv-opt /mnt/opt xfs defaults 0 0
    ```

24. Verify Mounts:

    ```
    sudo mount -a
    ```

25. Install NFS server, configure it to start on reboot and make sure it is u and running. Update the System and Install NFS Utilities:

    ```
    sudo yum -y update
    sudo yum install nfs-utils -y
    sudo systemctl start nfs-server.service
    sudo systemctl enable nfs-server.service
    sudo systemctl status nfs-server.service
    ```

26.  Export the NFS Mounts Use subnet cidr to connect as clients. For simplicity, we will install our all three Web Servers inside the same subnet, but in production set up would probably want to separate each tier inside its own subnet for higher level of security. To check our subnet cidr - open our EC2 details in AWS web console and locate Networking tab and open a Subnet link:

27. Set Permissions on Mount Points:

    ```
     sudo chown -R nobody:nobody /mnt/apps
     sudo chown -R nobody:nobody /mnt/logs
     sudo chown -R nobody:nobody /mnt/opt
     sudo chmod -R 777 /mnt/apps
     sudo chmod -R 777 /mnt/logs
     sudo chmod -R 777 /mnt/opt
    ```
    
28. Restart NFS Server
    ```
    sudo systemctl restart nfs-server.service
    ```

29. Configure access to NFS for clients within the same subnet (example of Subnet CIDR - 172.31.32.0/20 ):

    ```
    sudo vi /etc/exports
    ```

30. Add the following lines:

    ```
    /mnt/apps 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
    /mnt/logs 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
    /mnt/opt 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
    ```

31. Export the NFS Shares:

    ```
    sudo exportfs -arv
    ```


32. Check which port is used by NFS and open necessary ports in Security Groups (add new Inbound Rule). Check NFS Ports:

    ```
    rpcinfo -p | grep nfs
    ```

    image 7
    
### Important note: In order for NFS server to be accessible from our client,we open following ports: TCP 111, UDP 111, UDP 2049.


## Step 2 - Configure the database server

Create EC2 instance of t2.micro type with Ubuntu Server launch in the default region.

1. Update the server:

   ```
   sudo apt update -y
   ```

2. Install MySQL server:

   ```
   sudo apt install mysql-server

   ```
3. Check status:

   ```
   sudo systemctl status mysql
   ```
4. Enable MySQL to start on boot:

   ```
   sudo systemctl enable mysql
   ```
5. Create a database Tooling and User name it webaccess.

   ```
   sudo mysql
   CREATE DATABASE tooling;
   CREATE USER 'webaccess'@'%' IDENTIFIED BY 'mypass';
   GRANT ALL PRIVILEGES ON tooling. * TO 'webaccess'@'%' WITH GRANT OPTION;
   exit
   ```

##  Step 3 - Prepare the webservers

1. 





















            



    

     
     
                                                                        

    
    

    
    

      

























