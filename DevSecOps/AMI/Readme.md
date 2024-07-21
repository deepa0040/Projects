Copy AMI Backup To S3
Requirement:
S3 bucket should be in same region as AMI
EC2 Backup
 
S3 bucket (where backup will be stored)
 
Take S3 backup using the following command.

1.	Backup command

aws ec2 create-store-image-task --image-id ami-0a2a9fe858395ba4f --bucket ec2-mumbai-backup

 
2.	It take some time to take and store backup in S3 , Using following command you can see the progress.

aws ec2 describe-store-image-tasks 

 
3.	Restore data for S3.
aws ec2 create-restore-image-task --object-key ami-0a2a9fe858395ba4f.bin --bucket ec2-mumbai-backup --name "restore-script"

 

Limitation:
Please check limitation for this use case : https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ami-store-restore.html#limitations
•	To store an AMI, your AWS account must either own the AMI and its snapshots, or the AMI and its snapshots must be shared directly with your account. You can't store an AMI if it is only publicly shared.
•	Only EBS-backed AMIs can be stored using these APIs.
•	Paravirtual (PV) AMIs are not supported.
•	The size of an AMI (before compression) that can be stored is limited to 5,000 GB. 
•	Quota on store image requests: 600 GB of storage work (snapshot data) in progress. 
•	Quota on restore image requests: 300 GB of restore work (snapshot data) in progress. 
•	For the duration of the store task, the snapshots must not be deleted and the IAM principal doing the store must have access to the snapshots, otherwise the store process will fail. 
•	You can’t create multiple copies of an AMI in the same S3 bucket.
•	An AMI that is stored in an S3 bucket can’t be restored with its original AMI ID. You can mitigate this by using AMI aliasing.
•	Currently the store and restore APIs are only supported by using the AWS Command Line Interface, AWS SDKs, and Amazon EC2 API. You can’t store and restore an AMI using the Amazon EC2 console.
 
