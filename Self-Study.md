# My Self-Study while working on this project.


### 1. **NAS (Network Attached Storage):**
   - NAS is a storage device connected to a network that allows multiple users to access and share data. It's like having an external hard drive for an office where all the computers can access the same files.
   - **Protocols Used:**
     - **NFS (Network File System):** A protocol that lets computers access files over a network as if they were on their local storage. Commonly used in Linux environments.
     - **SMB (Server Message Block):** A protocol mostly used for sharing files, printers, etc., across Windows machines.
     - **SFTP (Secure File Transfer Protocol):** A secure version of FTP (File Transfer Protocol) for transferring files over a network using SSH for security.

### 2. **SAN (Storage Area Network):**
   - SAN is a specialized, high-speed network that provides block-level storage. It allows multiple servers to access shared storage as if it were directly attached to them. Think of it as a more advanced and faster way of connecting storage to servers compared to NAS.
   - **Protocols Used:**
     - **iSCSI (Internet Small Computer Systems Interface):** A protocol that allows SANs to use the TCP/IP network to transmit data over long distances.

### 3. **Block-Level Storage:**
   - Block-level storage means storing data in fixed-sized chunks called "blocks." It's commonly used for databases, virtual machines, and server storage. Each block is treated as a separate unit, which provides high flexibility for managing data.
   - **Used by Cloud Providers (like AWS):**
     - **AWS EBS (Elastic Block Store)** is a common example of block storage. It allows you to attach storage to your EC2 instances as if it were a physical hard drive.

### 4. **Object Storage:**
   - Object storage manages data as objects, which consist of the data itself, metadata, and a unique identifier. It's ideal for storing large amounts of unstructured data like photos, videos, backups, etc.
   - **Example in AWS:** 
     - **S3 (Simple Storage Service)** is a well-known object storage service used to store files, backups, and more. It's scalable and highly durable.

### 5. **Network File System (NFS):**
   - NFS, mentioned above under NAS, is a protocol used for sharing files over a network. In cloud computing, it’s used to mount a remote file system over a network. AWS provides **Amazon EFS (Elastic File System)** for this purpose.

### **Key Differences:**
   - **Block Storage vs. Object Storage:**
     - **Block Storage**: Best for high-performance applications (like databases). You can treat each block like a separate disk attached to your server.
     - **Object Storage**: Great for large-scale, unstructured data (like images, videos, backups) that doesn’t need to be frequently updated.
   - **Block Storage vs. NFS:**
     - Block storage is lower-level and usually faster, while NFS is used for file-level access across networks.

### **On AWS:**
   - **Block Storage**: AWS EBS (Elastic Block Store).
   - **Object Storage**: AWS S3 (Simple Storage Service).
   - **Network File System**: AWS EFS (Elastic File System).

# **Summary of Related Protocols:**
   - **NFS**: Used for sharing files across Linux systems.
   - **SMB**: Used for sharing files across Windows systems.
   - **SFTP**: Secure file transfer over a network.
   - **iSCSI**: Allows block-level storage over IP networks.

