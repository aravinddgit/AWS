
# AWS SAA-C03
**Source:** https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c03/

DOCUMENT REFERENCE: 

ppt: [AWS Certified Solutions Architect Slides v15.pdf](AWS_Certified_Solutions_Architect_Slides_v15.pdf)

## Introduction

### Questions

- How to create a user?
    - IAM > Access Mgmt > Users > Add users
- What is a group and how can u assign policies to a group and users to a group?
    - Add users > permissions options > add user to group > Create group > Group name | Attach required policies > Create user group > Next
    - or
    - IAM > Access Mgmt > User groups > Create group
- How to login as a user ?
    - Note down the accountID or the account alias from the root account. This can be done here: IAM > Dashboard >AWS Account (right-hand side) > Account ID | Account Alias. Then use the account alias, account ID, account name
- Policy structure ?
    - ppt
- how to remove users from group ?
    
    Users > Select user > Groups > Select group > Remove
    
    User groups > Select group > Users > Select user > Remove users
    
- how to create policies ?
    
    IAM > Policies > Create Policy > Create policy using visual editor or JSON
    
- how to add policies to user ?
    
    You can either add users to groups to add policies to the user 
    
    or
    
    We could directly add inline policies to a user: 
    
    Users > Select user > Permissions > Add permissions > Attach policies directly > Select policy > Next
    
    or
    
    Copy policies from another user:
    
    Users > Select user > Permissions > Add permissions > Copy permissions > Select user > Next
    
- how to create a custom password policy for all users?
    
    IAM > Access Mgmt > Account settings < Password policy > Edit
    
- how to add MFA to AWS account ?
    
    Username (top-right) > Security Credentials > Multi-factor authentication > Assign MFA device
    
- how to install aws-cli version 2
    - using msi installer
    - open terminal
    - aws —version
- how to create access keys for a user ?
    - root login
    - users tab
    - click on user
    - go to ‘security credentials’ tab
    - create access key
    - no tag info
    - save the access key and secret access key info
- Accessing aws-cli using admin1 access key
    - open terminal
    - aws configure
    - enter access key
    - enter secret access key
    - default region name
        - name of closest resgion - us-west-1
    - sample aws-cli command: aws iam list-users
        - if you do not have permission in the mgmt console, u will not have permission in the cli as well
    - code
        - 
        
        ```bash
        >aws --version
        aws-cli/2.11.3 Python/3.11.2 Windows/10 exe/AMD64 prompt/off
        
        >aws configure
        AWS Access Key ID [None]: xxx
        AWS Secret Access Key [None]: xxx
        Default region name [None]: xxx
        Default output format [None]:
        
        >aws iam list-users
        {
            "Users": [
                {
                    "Path": "/",
                    "UserName": "xxx",
                    "UserId": "xxx",
                    "Arn": "xxx",
                    "CreateDate": "2023-03-16T03:39:39+00:00",
                    "PasswordLastUsed": "2023-03-17T20:06:29+00:00"
                }
            ]
        }
        ```
        
- AWS Cloudshell
    - not available in all regions - check region list in documentation → reference guide → cloudshell ([https://docs.aws.amazon.com/general/latest/gr/cloudshell.html](https://docs.aws.amazon.com/general/latest/gr/cloudshell.html))
    - AWS CloudShell is a browser-based shell that gives you command-line access to your AWS resources in the selected AWS region. AWS CloudShell comes pre-installed with popular tools for resource management and creation. You have the same credentials as you used to log in to the console.
    - Basically, just login as the user into the AWS mgmt console. Just like we used access keys to run aws-cli commands from the windows terminal, we have cloud-shell for users in the aws mgmt console.
    - To use it, login as user in aws mgmt console, click on cloud-shell icon in the top bar
    - Pre-installed tools
    AWS CLI, Python, Node.js and more
    Storage included
    1 GB of storage free per AWS region
    Saved files and settings
    Files saved in your home directory are available in future sessions for the same AWS region
- IAM roles
    - To access AWS services to perform actions in an account. for eg, allows ec2 instances to call AWS services on your behalf
    - Basically, this is not created for physical users but for EC2 instances or lambda functions to access AWS services.
    - IAM > roles > create role
    - creating a role to allow ec2 instances to access aws services
        - trusted entity type = aws services | use case = EC2 → next → role details → role name | add permissions  → create role
- IAM Security tools
    - Credential Report
        - users and their passwords and access keys and other details etc.
        - IAM Side bar > Access reports > Credential report
    - Access Advisor
        - users and theirs service access status
        - can be used to remove permissions for those services that are not used frequently
        - users > <username> > access advisor tab
    

## EC2 Fundamentals, SAA Level and Instance Storage

- AWS Budget Setup
    - Initially IAM users do no have access to the Billing and Management Console (Account name > Billing Dashboard)
        
        
    - how to provide access to IAM users for billing and management console ?
        
        To activate IAM user and role access to the Billing and Cost Management console
        Sign in to the AWS Management Console with your root user credentials (specifically, the email address and password that you used to create your AWS account).
        
        On the navigation bar, choose your account name, and then choose Account.
        
        Next to IAM User and Role Access to Billing Information, choose Edit.
        
        Select the Activate IAM Access check box to activate access to the Billing and Cost Management console pages.
        
        Choose Update.
        
    - how to check charges by service?
        - Account name > Billing dashboard
        - left pane > bills
        - scroll down > charges by service tab
    - how to evaluate free tier usage?
        - Account name > Billing dashboard > left pane > free tier
    - how to create a budget?
        - Account name > Billing dashboard > left pane > budgets > create a budget
- EC2 Basics
    - What is EC2 ?
    - Capabilities of EC2 ?
    - Sizing and configuration options ?
    - what is EC2 user data ?
- how to create an EC2 instance with EC2 userdata
    - ec2 console > instances > Launch instance > configure > launch instance
- How to create a key-pair for an EC2 instance ?
    - There is an option to create a key-pair while creating an ec2 instance.
- What is the naming convention of EC2 instances ?
    - ppt
- What are the types of EC2 instances ?
    - ppt
- What are security groups on EC2 instances ?
    - ppt
- What do security groups regulate?
    - ppt
- How to create a security group
    - Security groups get created during the creation of an EC2 instance or
    - EC2 > Network & Security > Security Groups > Create Security groups
- How many security groups can be attached to an EC2 instance ?
    - Multiple
    - ppt
- Can the same security group be used in different regions ?’
    - No.
    - ppt
- Can the same security group be attached to multiple EC2 instances?
    - Yes, but only in the same region/VPC
- Where does security groups reside ? - Inside or outside an EC2 instance ?
    - Outside
    - ppt
- When does timeout occur and when does connection refused occur when facing an error while connecting to an EC2 instance through, say, HTTP?
    - Connection refused - application error
    - Timeout - Security Group
    - ppt
- Default inbound traffic rule ?
    - Block all
    - ppt
- Default outbound traffic rule ?
    - Allow all
    - ppt
- Default ports for SSH, HTTP, HTTPS, RDP, FTP, SFTP ?
    - ssh - 22
    - FTP , SFTP - 21
    - https - 443
    - RDP - 3389
    - ppt
- How to find out the security group attached to an instance ?
    - Select instance > Security > Check security group ID
- How to attach a security group for an EC2-instance ?
    - Every EC2 instance will have a default security group with inbound and outbound rules by default.
    - left pane > Network & Security > Security Groups > Select security group attached to the instance > actions > Edit inbound rules
    - or
    - EC2 > Network & Security > Security Groups > Create Security Group. Now EC2 > Instances > Right-click instance or click on Actions > Security > Change security groups
- How to connect to EC2-instance using SSH ? (how to modify permissions of .pem file)
    - Modifying permission for a .pem file
        - in Windows → right click on file → properties → Security → Advanced → Permissions → Add → Select a principal → Windows username
    - how to create a .pem file for an ec2-instance
        - ec2 > instances > Launch instance > key pair login > create new key pair - This will automatically download the new .pem file.
    - how to connect using windows powershell ?
        - ssh -i ‘<location of .pem file>’ <username>@<public_ip>
    - how to connect using EC2-instance connect ?
        - ec2 console > left pane > instances > select instance > Connect button > ec2 instance connect tab >
- How to access AWS services from an EC2 instance using EC2 Instance Roles
    - Connect to EC2 instance using EC2 instance connect
    - Now if u type say ‘aws iam list-users’ - it will throw an error in the console.
    - However, u could type ‘aws configure’ to login into a role and then perform the above command - but this is terrible practice as it makes the account vulnerable to attacks.
    - So, instead - do this → instances > select instance > actions or right-click instance > security > modify IAM role (say a role with iamreadacess policy attached to it)
    - Now go to EC2 instance connect’s console > run the same command as before - ‘‘aws iam list-users’’ - this will work now.
- EC2-purchasing options
    - ppt
    - On-Demand
    - Reserved / Convertible reserved
    - Savings
    - Spot
    - Dedicated Hosts / Instances
        
        Dedicated Instances and Dedicated Hosts are two options for customers who require a dedicated hardware instance for their workloads in AWS.
        
        Dedicated Instances
        Dedicated Instances provide instances that run on hardware that’s dedicated to a single AWS account, but they still share the underlying physical server with instances from other accounts. This means that although the hardware is dedicated, the physical server is still shared, and the instances on that server have access to the same resources, such as CPU, memory, and storage.
        
        Dedicated Instances can be useful for customers who require additional control over their instance placement in the underlying hardware, such as for compliance or regulatory reasons, or for customers who need to meet strict licensing requirements for certain software applications.
        
        The pricing for Dedicated Instances is higher than for On-Demand Instances, as customers pay a per-hour fee on top of the hourly usage fee for the EC2 instances they launch. This fee varies based on the instance type, region, and tenancy.
        
        In the context of Dedicated Instances, "hardware" refers to the physical resources (such as CPU, memory, and storage) that are allocated to a specific instance. This hardware is dedicated to a single AWS account, which means that it is not shared with other AWS customers.
        
        The Dedicated Instances are running on virtual machines, which are created and managed by the EC2 service. The physical hardware, in this case, refers to the physical servers that run the virtual machines.
        
        The key difference between hardware and physical server in this context is that the hardware refers to the underlying resources that are allocated to a specific instance, while the physical server refers to the physical machine on which the virtual machines are running.
        
        With Dedicated Instances, multiple instances from different AWS accounts can be running on the same physical server, but the hardware resources allocated to each instance are isolated and dedicated to that specific instance. This provides an additional layer of isolation between instances from different accounts, while still allowing for resource sharing to optimize resource utilization and cost efficiency.
        
        Dedicated Hosts
        Dedicated Hosts are physical servers with EC2 instance capacity fully dedicated to a single AWS account. This means that customers have full control over the underlying hardware, and they can launch instances onto a specific physical server that they have reserved. This allows customers to manage their own hardware inventory and placement, which can be useful for regulatory or compliance reasons, or for customers who need to meet strict licensing requirements for certain software applications.
        
        With Dedicated Hosts, customers pay for the entire server capacity upfront, regardless of how many instances they launch on it. This provides customers with cost savings for workloads that have predictable usage patterns or require long-term commitments.
        
        The pricing for Dedicated Hosts is based on the instance family, region, and billing option (on-demand or reserved), and customers are charged by the hour for the host capacity they reserve. There is also an option for customers to purchase Dedicated Hosts with a Savings Plan, which provides a significant discount on the hourly rate in exchange for committing to use the host for a one- or three-year term.
        
        It's important to note that while Dedicated Hosts offer the most control over the underlying hardware, they also come with additional responsibilities for customers, such as managing the server maintenance and ensuring that the instances launched on the host comply with any applicable licensing requirements.
        
    - Capacity Reservations
- How does a dedicated host differ from a dedicated instance?
    - Dedicated Instances provide instances that run on hardware that’s dedicated to a single AWS account, but they still share the underlying physical server with instances from other accounts. This means that although the hardware is dedicated, the physical server is still shared, and the instances on that server have access to the same resources, such as CPU, memory, and storage.
    - In the context of Dedicated Instances, "hardware" refers to the physical resources (such as CPU, memory, and storage) that are allocated to a specific instance. This hardware is dedicated to a single AWS account, which means that it is not shared with other AWS customers. The Dedicated Instances are running on virtual machines, which are created and managed by the EC2 service. The physical hardware, in this case, refers to the physical servers that run the virtual machines. The key difference between hardware and physical server in this context is that the hardware refers to the underlying resources that are allocated to a specific instance, while the physical server refers to the physical machine on which the virtual machines are running. With Dedicated Instances, multiple instances from different AWS accounts can be running on the same physical server, but the hardware resources allocated to each instance are isolated and dedicated to that specific instance. This provides an additional layer of isolation between instances from different accounts, while still allowing for resource sharing to optimize resource utilization and cost efficiency.
    - Also, you cannot directly add or modify the hardware resources allocated to Dedicated Instances. The hardware resources that are allocated to a Dedicated Instance are fixed and cannot be changed while the instance is running.
    - Dedicated Instances can be useful for customers who require additional control over their instance placement in the underlying hardware, such as for compliance or regulatory reasons, or for customers who need to meet strict licensing requirements for certain software applications. The pricing for Dedicated Instances is higher than for On-Demand Instances, as customers pay a per-hour fee on top of the hourly usage fee for the EC2 instances they launch. This fee varies based on the instance type, region, and tenancy.
    - when using Dedicated Instances in Amazon EC2, you do have control over instance placement to a certain extent. With Dedicated Instances, you have the ability to specify the instance placement at the regional level. When you launch a Dedicated Instance, you can choose the Availability Zone within a region where the instance will be placed. This provides you with control over the placement of your instances to meet specific requirements or preferences. For example, you can select a specific Availability Zone to meet compliance or regulatory requirements or to satisfy proximity constraints for your workload. However, it's important to note that within the selected Availability Zone, the specific underlying hardware on which your Dedicated Instance runs is managed by AWS. You don't have direct control or visibility over the physical server or hardware allocation. AWS ensures that your Dedicated Instances are isolated from instances of other AWS accounts at the hardware level, maintaining the dedicated tenancy.
    - Dedicated hosts
    - Dedicated Hosts are physical servers with EC2 instance capacity fully dedicated to a single AWS account. This means that customers have full control over the underlying hardware, and they can launch instances onto a specific physical server that they have reserved. This allows customers to manage their own hardware inventory and placement, which can be useful for regulatory or compliance reasons, or for customers who need to meet strict licensing requirements for certain software applications.
    - With Dedicated Hosts, customers pay for the entire server capacity upfront, regardless of how many instances they launch on it. This provides customers with cost savings for workloads that have predictable usage patterns or require long-term commitments.
    - The pricing for Dedicated Hosts is based on the instance family, region, and billing option (on-demand or reserved), and customers are charged by the hour for the host capacity they reserve. There is also an option for customers to purchase Dedicated Hosts with a Savings Plan, which provides a significant discount on the hourly rate in exchange for committing to use the host for a one- or three-year term.
    - It's important to note that while Dedicated Hosts offer the most control over the underlying hardware, they also come with additional responsibilities for customers, such as managing the server maintenance and ensuring that the instances launched on the host comply with any applicable licensing requirements.
- What are capacity reservations?
    - EC2 capacity reservations are a feature in Amazon EC2 that allow you to reserve and ensure capacity for EC2 instances in a specific Availability Zone within a region. By creating a capacity reservation, you guarantee that the desired instance type and quantity will be available for your use.
    - Here are some key points to understand about EC2 capacity reservations:
        - Reservation Scope: Capacity reservations are created within a specific Availability Zone of a region. They are not cross-Availability Zone or cross-region.
        - Instance Type and Quantity: When creating a capacity reservation, you specify the desired instance type and the number of instances you want to reserve. This ensures that the specified capacity is reserved exclusively for your use.
        - Instance Availability: Capacity reservations provide capacity guarantees for instances but not instance launch guarantees. This means that you have ensured capacity, but you still need to launch instances within the capacity reservation.
        - Usage: Capacity reservations can be used for On-Demand instances or as a pool for Reserved Instances. This means you can either use them for pay-as-you-go instances or assign the capacity reservation to Reserved Instances, allowing you to benefit from reserved instance pricing.
        - Instance Placement: When launching instances within a capacity reservation, you have the option to explicitly target the capacity reservation, ensuring that the instances are launched within the reserved capacity.
    - EC2 capacity reservations are useful for applications or workloads that require specific capacity and availability guarantees. By reserving capacity, you can have confidence in launching the required instances whenever you need them, ensuring the availability of resources for your workload.
    - **It's important to note that EC2 capacity reservations are separate from other EC2 purchasing options like On-Demand instances, Spot instances, or Reserved Instances. They provide a specific capacity guarantee within an Availability Zone and can be used in conjunction with other purchasing options as needed.**
- Spot Instance Requests
    - What is a spot instance?
        
        ppt
        
    - What is the procedure to remove spot instance ?
        - First cancel the spot request and then terminate the associated instances - in the case of a persistent spot request.
    - When can you cancel spot requests ?
        - When the instance is in open, active or disabled state (not in closed state)
    - ppt
- What are spot fleets ?
    - ppt
- What are the strategies to allocate spot fleets ?
    - ppt data is outdated for this question
    - Price-capacity optimized
    - Capacity optimized
- How to launch a spot instance?
    - Instances > launch instance > check the box corresponding to spot instance under ‘advanced’
- How to launch a spot fleet
    - ec2 console > spot requests > request spot instance | customize
- How to launch reserved instances/dedicated hosts/capacity reservations?
    - side pane
    
- Private vs Public vs Elastic IP
- How to associate a elastic IP to an instance
    - Go to ‘Elastic IPs’ tab in the side tab
    - Click on ‘Allocate Elastic IP Address’ > select instance > select private IP > Done
    - Note: Unused elastic IPs are usually charged
- What do we connect to an instance using an Elastic IP?
    
    
- How to dissociate an elastic IP from an instance ?
    - ‘Elastic IP’ > Select IP > Dissociate Elastic IP Address
- How to release an elastic IP ?
    - Select Elastic IP in ‘Elastic IPs’ page | Actions > Release Elastic IP addresses
    - Unreleased and unassociated Elastic IP addresses will be charges. Always make sure that unused Elastic IPs are released.
- What are placement groups?
- Types of placement groups?
    1. Cluster placement group: All instances are present in the same hardware rack in the same AZ. For application with low network latency and high throughput
    2. Partition placement group: Some instances are in the same rack while some others in a different rack within or across AZs. Limitation: max 7 hardware rack partitions in any AZ. For applications designed for large distributed and parallel computing workloads. Example: large-scale scientific simulations and data analytics workloads. Other examples: Cassandra, Kafka, HDFS
    3. Spread placement group: Each instance is in a different rack within or across AZs. Limitation: Max 7 instances within an AZ (in one placement group)
- How to create a placement group?
    - Side menu > Network & Security > Placement Groups > Create Placement Group
- How to associate an instance with a placement group?
    - Instances > Launch instance > Advanced Details > ‘Placement Group’ dropdown
- What is an ENI?
    - Creating a secondary IPV4 for an EC2 instance
    - [https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/](https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/)
    - Provides an ability to create EC2 instances in multiple VPCs or subnets.
- How do we create an ENI?
    - EC2 > Network & Security > Network Interfaces > Create network interface > select subnet according to the region of the ec2 instance > select a security group > create
- How can we attach a secondary ENI to an EC2 instance ?
    - Select the ENI > actions > attach > select instance > attach
- How do we “move” an ENI from one instance to another?
    - Select ENI > Actions > Attach
- What is EC2 Hibernate ?
    - PPT
- how to enable EC2 hibernate ?
    - Instances > Launch instance > Advanced Details > Stop- Hibernate behaviour > Enable
    - Also, While hibernate, we need to ensure that the EBS root volume is encrypted and has storage size > RAM size. So, while launching also do the following:
    - Storage (volumes) > EBS Volumes > Encrypted > Encrypted | KMS Key > Default
    - Launch instance
- how to hibernate instance?
    - instances > select instance > instance state > hibernate instance
- How is hibernating different from Stopping instance
    - During hibernating, the RAM is stored in EBS volume and restored back from EBS into the RAM during start
    - From an OS perspective, the instance acts as though the instance was never stopped ie it continues from the last known state before hibernation


### EC2 Instance Storage