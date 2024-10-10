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
   
4. List the disk:

   ```
   lsblk
   ```

5. Use df -h command to see all mounts and free space:

   ```
   df -h
   ```

6. Use gdisk utility to create a single partition on each of the 3 disks:

   ```
   sudo gdisk /dev/xvdf
   ```

7. Install lvm2 package: creating logical volumes, which can be resized or moved without needing to unmount file systems.

   ```
   sudo yum install lvm2 -y
   ```

8. Check for available partitions.

   ```
   sudo lvmdiskscan
   ```      

9. Create Physical Volumes Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

    ```
    sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
    ```

10.     


























