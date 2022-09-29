# VPC - Setup and Test Connection

The diagram attached below;

![VPC Secure access](https://user-images.githubusercontent.com/86754468/192643693-298bef0c-1c45-4190-b70f-d67be7979b39.png)


First step to do is creating **`VPC`** then apply the necessary components of VPC that are as follows;
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#creating-vpc" target="_blank">*Virtual Private Cloud(VPC)*</a>
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#creating-subnets" target="_blank">*Subnets*</a>
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#creating-internet-gatewayigw" target="_blank">*Internet Gateways (igw)*</a>
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#creating-route-tables" target="_blank">*Route Tables*</a>
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#creating-nat-gateways" target="_blank">*NAT Gateways*</a>
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#creating-network-alcsnacl" target="_blank">*Network ACLs*</a>
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#creating-security-groups" target="_blank">*Security Groups*</a>
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#spin-vm---launch-instance" target="_blank">*Launch Instance*</a> 
- <a href="https://github.com/githabocal/vpc/blob/vpc_setup/README.md#testing-connectivity" target="_blank">*Testing Connectivity*</a>  
  
**Notes:** 
- Bastion-host is our VM - Bastion host has been used for private connectivity from public to private
- NACL works in subnet level; not EC2 level

# <h3>Creating VPC:
- Head to Your VPCs in AWS and choose **`Create VPC`** on the top right of the page
- **`VPC only`** must be selected for **`resources to create`**
- Set a name for VPC then enter **`IPv4 CIDR`** as **`192.168.0.0/24`**. We prefer **`/24`** as **_CIDR_** because it is more secure and number of IPs limited
- **`Tenancy`** must be selected as **`Default`**; _`NOT Dedicated`_
- Then click on **`Create VPC`**
- Now **`VPC`** is created successfully
- Next step is creating **`Subnets`**

<a href="#top">Back to top</a>

# <h3>Creating Subnets:
- Head to **`Subnet`** under `Virtual private cloud` dropdown and click on **`Create subnet`** on the top right of the page
- Choose the created VPC for **VPC ID**
- Then set as follows;
  - `subnet name`: **`public-1`**
  - `Availability zone`: **`us-east-1a`** 
  - `IPv4 CIDR block`: **`192.168.0.0/26`**
  - then click on **`Add new subnet`** to add second public subnet
- Then set as follows;
  - `subnet name`: **`public-2`**
  - `Availability zone`: **`us-east-1b`** 
  - `IPv4 CIDR block`: **`192.168.0.64/26`**
  - then click on **`Add new subnet`** to add first private subnet
- Then set as follows;
  - `subnet name`: **`private-1`** 
  - `Availability zone`: **`us-east-1a`** 
  - `IPv4 CIDR block`: **`192.168.0.128/26`**
  - then click on **`Add new subnet`** to add second private  subnet
- Then set as follows;
  - `subnet name`: **`private-2`** 
  - `Availability zone`: **`us-east-1b`**
  - `IPv4 CIDR block`: **`192.168.0.192/26`**
  - then click on **`Create subnet`** button
- 2(two) public and 2(two) private subnets have been created successfully!
- After all, select subnet **`public-1`** and click on `Actions` dropdown and choose **`Edit subnet settings`** and click on `Enable auto-assign public IPv4 address` and then **`Save`**

<a href="#top">Back to top</a>

# <h3>Creating Internet Gateway(igw):
- Head to **`Internet gateways`** under `Virtual private cloud` dropdown and click on **`Create internet gateway`** on the top right of the page
- Then set a name for internet gateway and click on **`Create internet gateway`**
- After creating a internet gateway, we must attach it to the **`created VPC`** and thus click on **Actions** dropdown and choose **`Attach to VPC`** then select the **`created VPC`** and click on **`Attach internet gateway`**
- Then once you head to internet gateways, you must be able to verify that **`State is Attached`**

<a href="#top">Back to top</a>

# <h3>Creating Route Tables:
- Head to **`Route tables`** under `Virtual private cloud` dropdown and click on **`Create route table`** on the top right of the page
- Then set a name for public - route table and choose the selected VPC then click on **`Create route table`**
- Then head to **`Edit routes`** and click on **`Add route`** and set destination as **`0.0.0.0/0`** and target as the **`created internet gateway`** and click on **`Save changes`**
- Then head to **`Subnet associations`** and click on **`Edit subnet associations`** and choose *all public subnets* which are *`public-1`* and *`public-2`* and then click on **`Save associations`**
- And now, we must create another route table for private but before that, we must create **`Network ACLs`**
- Once we create private route table, we must follow the same steps for private. The difference in private route table, while editing route, we must set destination as **`0.0.0.0/0`** same as public but target must be the **`created nat gateway`**

<a href="#top">Back to top</a>

# <h3>Creating NAT gateways:
- Head to **`NAT gateways`** under `Virtual private cloud` dropdown and click on **`Create NAT table`** on the top right of the page
- Set a name and select the proper subnet which is `public-2` for the current diagram then click on `Allocate Elastic IP` to assign an Elastic IP address to the NAT gateway and then click on **`Create NAT gateway`**

<a href="#top">Back to top</a>

# <h3>Creating Network ALCs(NACL):
- Head to **`Network ACLs`** under `Security` dropdown and click on **`Create network ACL`** on the top right of the page
- Then set a name as **`NACL-public-1`** and choose the selected VPC then click on **`Create network ACL`**
- And now, we must modify the created NACL, thus we need to head to **`Inbound rules`** and click on **`Edit inbound rules`** then click on **`Add new rule`**
- Set as follows;
    - `Rule Number`: **`110`**
    - `Port range`: **`22`**
    - `Source`: **local public IP** - **`108.53.13.199/32`**
    - then click on **`Save changes`**
- Head to **`Outbound rules`** and click on **`Edit outbound rules`** the click on **`Add new rule`**
- Set as follows;
    - `Rule number`: **`110`**
    - `Port range`: **`1024-65535`** 
    - `Source`: **local public IP** - **`108.53.13.199/32`**
    - and then click on **`Save changes`**
- Click on **`Create network ACL`** to create private NACL then set a name and choose the selected VPC and click on **`Create network ACL`**
- And now, we must modify the created NACL, thus we need to head to **`Inbound rules`** and click on **`Edit inbound rules`** then click on **`Add new rule`**
- Set as follows;
    -  `Rule Number`: **`110`**
    -  `Port range`: **`22`**
    -  `Source`: **`192.168.0.0/24`**
    -  and then click on **`Add new rule`**
- And set as follows;
    - `Rule Number`: **`120`**
    - `Type`: **`Custom ICMP - IPv4`** 
    - and then click on **`Add new rule`** 
- And set as follows;
    -  `Rule Number`: **`130`**
    -  `Port range`: **`1024-65535`** 
    -  and then click on **`Save changes`**
- After editing the `inbound rules`, now we must also edit `outbound rules` thus head to `Outbound rules` and click on **`Edit outbound rules`** and click on **`Add new rule`**
- Then set as follows;
    - `Rule Number`: **`110`**
    - `Type`: **`HTTPS(443)`** 
    - and then click on **`Add new rule`** 
- And set as folows;
    -  `Rule Number`: **`120`**
    -  `Type`: **`Custom ICMP - IPv4`** 
    -  and then click on **`Add new rule`** 
- And set as follows
    -  `Rule Number`: **`130`** 
    -  `Type`: **`Custom TCP`** 
    -  `Port range`: **`1024-65535`** 
    -  and then click on **`Save changes`**
- And now we are good to go for `Subnet associations` and click on **`Edit subnet association`** and select `private subnets` and click on **`Save changes`**
- We also need to create a NACL for NAT gateway thus head to **`*Create network ACL`** 
- And set a name and choose the selected VPC and then click on **`Create network ACL`**
- Then set as follows;
    -  `Rule Number`: **`110`**
    -  `Type`: **`HTTPS(443)`**
    -  `Source`: **`192.168.0.0/24`** 
    -  and then click on **`Add new rule`**   
-  And set as follows;
   -  `Rule Number`: **`120`** 
   -  `Type`: **`Custom TCP`** 
   -  `Port range`: **`1024-65535`** 
   -  and then click on **`Add new rule`** 
-  And set as follows;
   -  `Rule Number`: **`130`**
   -  `Type`: **`Custom ICMP - IPv4`** 
   -   and click on **`Save changes`**
- Then we must also edit Outbound rules thus head to `Edit outbound rules` and click on `Add new rule`
- Then set as follows;
    - `Rule Number`: **`110`** 
    - `Type`: **`HTTPS(443)`** 
    - and then click on **`Add new rule`** 
- And set as follows;
    - `Rule Number`: **`120`**  
    - `Type`: **`Custom TCP`**  
    - `Port range`: **`1024-65535`** 
    - `Destination`: **`192.168.0.0/24`** 
    - and then click on **`Add new rule`** 
- And set as follows;
    -  `Rule Number`: **`130`** 
    -  `Type`: **`Custom ICMP - IPv4`** 
    -  and click on **`Save changes`**
- Then head to Subnet associations and click on **`Edit subnet association`** and select `public-2` 
- And then click on **`Save changes`**

<a href="#top">Back to top</a>
  
# <h3>Creating Security Groups:
-  Head to **`Security groups`** under `Security` dropdown and click on **`Create security group`** on the top right of the page 
-  Set a security group name for `public` and choose the selected VPC 
-  and then click on **`Add rule`** for `Inbound rules` 
-  then set as follows;
   -  `Port range`: **`22`** 
   -  `Source`: **`My IP`** 
   -  then click on **`Create security group`**
-  And we must also create an another security group for `private` and click on **`Create security group`** and set a security group name for `private` and choose the selected VPC and then click on **`Add rule`** for `Inbound rules` 
-  then set as follows;
   -  `Port range`: **`22`**  
   -  `Source`: **`192.168.0.0/24`** 
   -  then click on **`Create security group`**
-  Then head to Outbound rules and click on **`Edit outbound rules`** and click on `Add rule` and select **`HTTPS`** and **`All ICMP - IPv4`** for `Type` and **`0.0.0.0/0`** for `Destination` and then click on **`Save rules`**
- The security group will be used for `Network Setting in EC2`

<a href="#top">Back to top</a>

# <h3>Spin VM - Launch Instance:
- Head to **`EC2`** in AWS then click on **`Launch Instance`**
- Set a name for EC2 and scroll down to **`Key pair`** then click on **`Create new key pair`**
- Set a key pair name and `Key pair type` and `Private key file format` must be selected **`RSA`** and **`.pem`** respectively
- Then click on **`Create key pair`**
- In Network Setting, click on **`Edit`** and choose the selected VPC then pick the `subnet` for **`public-1`**
- Then select **`Select existing security group`** and choose the `created security group` in Common security groups
- Then click on **`Launch instance`**
- And `Launch instance` must be initiated successfully!
- Then click on `View all instances` to visualize the instance and its status
- Make sure that `Instance state` is **`Running`**
- Now we must create another instance for `private` and click on **`Launch instance`** and set a name and scroll down to **`Key Pair`** and choose **`bastion`** which is already created 
- In Network Setting, click on **`Edit`** and choose the selected VPC then pick the `subnet` for **`private-1`** and Auto-assign public IP must be selected `Disable` by default
- Then select **`Select existing security group`** and choose the `created security group for private` in Common security groups
- Then click on **`Launch instance`**
- Then click on `View all instances` to visualize the instance and its status
- Make sure that `Instance state` is **`Running`**
- **Now we are ready to test connectivity!**
  
<a href="#top">Back to top</a>

# <h3>Testing Connectivity:
- Select the created instance and copy the **`Public IPv4 address`**
- Open the terminal and run `ssh -i ~/Downloads/bastion.pem ec2-user@<the copied public IPv4 address>`
- If you have permission issue due to read and write, please run `chmod 400 ~/Downloads/bastion.pem` to update your permission with **_`READ ONLY`_** then run `ssh -i ~/Downloads/bastion.pem ec2-user@<the copied public IPv4 address>` again to verify the connectivity!
- In addition, you may also run `whoami` to verify **`ec2-user`**
- After we created ec2 instance for private, we would **_NOT_** able to connect with the same way for **`Private IPv4`** for the instance that created for private. To achieve it, we need to do as follows;
    - Open the terminal and run `ssh ec2-user@<Private IPv4 of Private ec2 instance>` once logged into Bastion
  
  **_Note: To run ssh, we may also follow the steps below;_** 
    - Open the terminal and run the following commands;
    - `ssh-add -K ~/<Path of .pem file>` --> adds pem file to ssh identity 
    - `ssh -A ec2-user@<Public IPv4>
IPv4 for public ec2 instance>` --> Login to Instance
    - Then run `ip addr` and copy the ONLY IP and paste it search box in EC2 console and verify instance name with IP.

- At the end, we may run `ping 8.8.8.8` or `ping google.com` or `ping -c 3 amazon.com` in the terminal and verify **`0% packet loss`**
  
  <img width="472" alt="Screen Shot 2022-09-27 at 1 45 25 AM" src="https://user-images.githubusercontent.com/86754468/192643386-e6cecddd-6db6-40ea-9adb-b61ed2fcac48.png">

  ![Screen Shot 2022-09-27 at 2 25 03 AM](https://user-images.githubusercontent.com/86754468/192643157-9a096161-3ba7-4fea-8419-ff395076c5cf.png)

  ![Screen Shot 2022-09-27 at 2 25 18 AM](https://user-images.githubusercontent.com/86754468/192643218-3c3c6288-f3a9-423b-bf87-3348c641a198.png)


  <a href="#top">Back to top</a>
