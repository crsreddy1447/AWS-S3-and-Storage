# AWS-S3-and-Storage

#### AWS S3
Amazon S3 
Amazon Simple Storage Service: has a simple web services interface that you can use to store and retrieve any amount of data, at any time, from anywhere on the web.
Used to create buckets, store and retrieve your objects, and manage permissions on your resources.

***Amazon S3 objects can range in size from a minimum of 0 bytes to a maximum of 5 terabytes. 
The largest object that can be uploaded in a single PUT is 5 gigabytes.


Buckets
A bucket is a container for objects stored in Amazon S3. Every object is contained in a bucket.

Amazon S3 offers a range of storage classes designed for different use cases.

Storage Classes for Frequently Accessed Objects
1. S3 Standard:  The default storage class.
2. Reduced Redundancy: storage class is designed for noncritical, reproducible data. Not recommended

Storage Class for Automatically Optimizing Frequently and Infrequently Accessed Objects

1. S3 Intelligent-Tiering storage class: is designed to optimize storage costs by automatically moving data to the most cost-effective storage access tier, without performance impact or operational overhead.
There are no retrieval fees when using the S3 Intelligent-Tiering storage class. 

Storage Classes for Infrequently Accessed Objects (IA-infrequent access)
For older data that is accessed infrequently, but that still requires millisecond access.
1. S3 Standard-IA: Amazon S3 stores the object data redundantly across multiple geographically separated Availability Zones

2. S3 One Zone-IA: Amazon S3 stores the object data in only one Availability Zone, which makes it less expensive than S3 Standard-IA.

Storage Classes for Archiving Objects
These storage classes are designed for low-cost data archiving.
1. S3 Glacier: Use for archives where portions of the data might need to be retrieved in minutes. Data stored in the S3 Glacier storage class has a minimum storage duration period of 90 days and can be accessed in as little as 1-5 minutes using expedited retrieval.

2. S3 Glacier Deep Archive: Use for archiving data that rarely needs to be accessed. Data stored in the S3 Glacier Deep Archive storage class has a minimum storage duration period of 180 days and a default retrieval time of 12 hours.
**The S3 Glacier or S3 Glacier Deep Archive storage classes are not accessible in real time. You must first initiate a restore request, and then wait until a temporary copy of the object is available for the duration


Bucket policies
Bucket policies provide centralized access control to buckets and objects based on a variety of conditions, including Amazon S3 operations, requesters, resources, and aspects of the request

Access control lists
Access control lists (ACLs): are one of the resource-based access policy options that you can use to manage access to your buckets and objects.

Versioning
Versioning allows you to preserve, retrieve, and restore every version of every object stored in an Amazon S3 bucket. Once you enable Versioning for a bucket, Amazon S3 preserves existing objects anytime you perform a PUT, POST, COPY, or DELETE operation on them. By default, GET requests will retrieve the most recently written version.
S3 Versioning protects you from the consequences of unintended overwrites and deletions. You can also use it to archive objects so that you have access to previous versions.

***Amazon S3 Standard, S3 Standard-Infrequent Access, and S3 Glacier storage classes replicate data across a minimum of three AZs to protect against the loss of one entire AZ.

S3 Access Point is configured with an access policy specific to a use case or application, and a bucket can have hundreds of access points.
A bucket is the logical storage container for your objects while an access point provides access to the bucket and its contents.


##### AWS S3 CLI

mb: Creates an S3 bucket
aws s3 mb s3://mybucket
aws s3 mb s3://mybucket --region us-west-1

cp: Copies a file 
aws s3 cp test.txt s3://mybucket/test2.txt    ------> Copying a local file to S3
aws s3 cp test.txt s3://mybucket/test2.txt --expires 2014-10-01T20:30:00Z    ----> Copying a local file to S3 with an expiration date
aws s3 cp s3://mybucket/test.txt s3://mybucket/test2.txt    ------> Copying a file from S3 to S3
aws s3 cp s3://mybucket/test.txt test2.txt    -----> Copying an S3 object to a local file
aws s3 cp s3://mybucket . --recursive

mv: Moves a local file or S3 object
aws s3 mv test.txt s3://mybucket/test2.txt    ----->moves a single file to a specified bucket
aws s3 mv s3://mybucket/test.txt s3://mybucket/test2.txt     -----> moves a single s3 object to a specified bucket
aws s3 mv s3://mybucket/test.txt test2.txt  -----> moves a single object to a specified file locally
aws s3 mv s3://mybucket . --recursive     --------> recursively moves all objects 
aws s3 mv myDir s3://mybucket/ --recursive --exclude "*.jpg"

rb: Deletes an empty S3 bucket
aws s3 rb s3://mybucket
aws s3 rb s3://mybucket --force

rm: Deletes an S3 object
aws s3 rm s3://mybucket/test2.txt    -----> deletes a single s3 object
aws s3 rm s3://mybucket --recursive   -----> recursively deletes all objects under a specified bucket 
aws s3 rm s3://mybucket/ --recursive --exclude "*.jpg"    -----> deletes all objects while excluding some objects to keep.

sync: Syncs directories and S3 prefixes
aws s3 sync . s3://mybucket   ---->  syncs the bucket mybucket to the local current directory
aws s3 sync s3://mybucket s3://mybucket2   ----->syncs the bucket mybucket to the bucket mybucket2
aws s3 sync s3://mybucket .    ------> syncs the current local directory to the bucket mybucket

website: Set the website configuration for a bucket
aws s3 website s3://my-bucket/ --index-document index.html --error-document error.html



=====================================================================================================================


#### AWS EBS and AWS EFS
STORAGES of AWS
Amazon S3
Amazon Elastic Block Store
Amazon Elastic File System
Amazon FSx for Lustre
Amazon FSx for Windows File Server
Amazon S3 Glacier
AWS Storage Gateway

Amazon Elastic Block Store and Amazon Elastic File System and AWS Storage Gateway

Amazon Elastic Block Store: 
Amazon Elastic Block Store (Amazon EBS) provides block level storage volumes for use with EC2 instances.
Multiple volumes to one instance and vice versa
EBS volumes are created in a specific Availability Zone, and can then be attached to any instances in that same Availability Zone. To make a volume available outside of the Availability Zone, you can create a snapshot and restore that snapshot to a new volume
Amazon EBS provides the following volume types: 
1. General Purpose SSD base performance of 3 IOPS/GiB, with the ability to burst to 3,000 IOPS for extended periods of time.
volume can range in size from 1 GiB to 16 TiB.

2. Provisioned IOPS SSD: support up to 64,000 IOPS and 1,000 MiB/s of throughput
Provisioned IOPS SSD (io1) volumes are designed to meet the needs of I/O-intensive workloads, particularly database workloads

3. Throughput Optimized HDD: provide low-cost magnetic storage that defines performance in terms of throughput rather than IOPS.
4. Cold HDD: low-cost magnetic storage that defines performance in terms of throughput rather than IOPS
5. Magnetic

### AWS CLI EBS
aws ec2 create-volume --volume-type gp2 --size 80 --availability-zone us-east-1a
aws ec2 attach-volume --volume-id vol-1234567890abcdef0 --instance-id i-01474ef662b89480 --device /dev/sdf
aws ec2 describe-volumes


### Amazon Elastic File System:

Amazon Elastic File System (Amazon EFS) is a fully-managed service that provides a simple, scalable, fully managed, elastic NFS file system. It is built to scale on demand to petabytes without disrupting applications, growing and shrinking automatically as you add and remove files.
Amazon EFS file systems can automatically scale from gigabytes to petabytes of data without needing to provision storage.

EFS offers two storage classes: 
Standard storage class, and 
Infrequent Access storage class (EFS IA)

To access your file system, you mount the file system on an Amazon EC2 Linux-based instance using the standard Linux mount command and the file system’s DNS name. 
Amazon EFS uses the Network File System version 4 (NFS v4) protocol. 
AWS DataSync provides a fast and simple way to securely sync existing file systems with Amazon EFS.
Amazon EFS supports 1000 Amazon EC2 instances connecting to a file system concurrently.
You can create up to 1,000 file systems per region.


#### AWS Storage Gateway
AWS Storage Gateway is a hybrid cloud storage service that gives you on-premises access to virtually unlimited cloud storage.
The gateway connects to AWS storage services. The service includes a highly-optimized data transfer mechanism, with bandwidth management, automated network resilience, and efficient data transfer.

The service includes a highly-optimized data transfer mechanism, with bandwidth management, automated network resilience, and efficient data transfer.

File Gateway:  File Gateway presents a file-based interface to Amazon S3, which appears as a network file share. It enables you to store and retrieve Amazon S3 objects through standard file storage protocols.

Tape Gateway: Tape Gateway is a cloud-based Virtual Tape Library (VTL). It presents your backup application with a VTL interface, consisting of a media changer and tape drives. You can create virtual tapes in your virtual tape library using the AWS Management Console. 

Volume Gateway: Volume Gateway provides an iSCSI target, which enables you to create block storage volumes and mount them as iSCSI devices from your on-premises or EC2 application servers. The Volume Gateway runs in either a cached or stored mode.


