# VPC - Setup and Test Connection

First step to do is creating **`VPC`** then apply the necessary components of VPC that are as follows;
- *Subnets*
- *Internet Gateways (igw)*
- *Route Tables*
- *NAT Gateways*
- *Network ACLs*
  
Notes: 
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

# <h3>Creating Subnets:
- Head to **`Subnet`** under `Virtual private cloud` dropdown and click on **`Create subnet`** on the top right of the page
- Choose the created VPC for **VPC ID**
- Then set a subnet name for *`public-1`* and availability zone **`us-east-1a`** then choose **`IPv4 CIDR block`** as **`192.168.0.0/26`**
- Then click on **`Add new subnet`** to add second public subnet
- Then set a subnet name for *`public-2`* and availability zone **`us-east-1b`** then choose **`IPv4 CIDR block`** as **`192.168.0.64/26`**
- Then click on **`Add new subnet`** to add first private subnet
- Then set a subnet name for *`private-1`* and availability zone **`us-east-1a`** then choose **`IPv4 CIDR block`** as **`192.168.0.128/26`**
- Then click on **`Add new subnet`** to add second private subnet
- Then set a subnet name for *`private-2`* and availability zone **`us-east-1b`** then choose **`IPv4 CIDR block`** as **`192.168.0.192/26`**
- Then click on **`Create subnet`** button
- 2(two) public and 2(two) private subnets have been created successfully!
- After all, select subnet **`public-1`** and click on `Actions` dropdown and choose **`Edit subnet settings`**

# <h3>Creating Internet Gateway(igw):
- Head to **`Internet gateways`** under `Virtual private cloud` dropdown and click on **`Create internet gateway`** on the top right of the page
- Then set a name for internet gateway and click on **`Create internet gateway`**
- After creating a internet gateway, we must attach it to the **`created VPC`** and thus click on **Actions** dropdown and choose **`Attach to VPC`** then select the **`created VPC`** and click on **`Attach internet gateway`**
- Then once you head to internet gateways, you must be able to verify that **`State is Attached`**

# <h3>Creating Route Tables:
- Head to **`Route tables`** under `Virtual private cloud` dropdown and click on **`Create route table`** on the top right of the page
- Then set a name for public - route table and choose the selected VPC then click on **`Create route table`**
- Then head to **`Edit routes`** and click on **`Add route`** and set destination as **`0.0.0.0/0`** and target as the **`created internet gateway`** and click on **`Save changes`**
- Then head to **`Subnet associations`** and click on **`Edit subnet associations`** and choose *all public subnets* which are *`public-1`* and *`public-2`* and then click on **`Save associations`**
- And now, we must create another route table for private but before that, we must create **`Network ACLs`**

# <h3>Creating Network ALCs(NACL):
- Head to **`Network ACLs`** under `Security` dropdown and click on **`Create network ACL`** on the top right of the page
- Then set a name as **`NACL-public-1`** and choose the selected VPC then click on **`Create network ACL`**
- And now, we must modify the created NACL, thus we need to head to **`Inbound rules`** and click on **`Edit inbound rules`** then click on **`Add new rule`**
- Set `Rule Number` as **`110`**, and update **`Port range`** with **`22`** and we use our **local public IP - `108.53.13.199/32` - for `Source`** and the click on **`Save changes`**
- Head to **`Outbound rules`** and click on **`Edit outbound rules`** the click on **`Add new rule`**
- Set `Rule number` as **`110`**, and update **`Port range`** with **`1024-65535`** and we use our **local public IP - `108.53.13.199/32` - for `Source`** and the click on **`Save changes`**
- Click on **`Create network ACL`** to create private NACL then set a name and choose the selected VPC and click on **`Create network ACL`**
- And now, we must modify the created NACL, thus we need to head to **`Inbound rules`** and click on **`Edit inbound rules`** then click on **`Add new rule`**
- Set `Rule Number` as **`110`**, and update **`Port range`** with **`22`** and `192.168.0.0/24` - for `Source`** and the click on **`Add new rule`**
- And set `Rule Number` as **`120`**, and update **`Type`** with **`Custom ICMP - IPv4`** and click on **`Add new rule`** set `Rule Number` as **`130`**, and update **`Port range`** with **`1024-65535`** then click on **`Save changes`**
- After editing the `inbound rules`, now we must also edit `outbound rules` thus head to `Outbound rules` and click on **`Edit outbound rules`** and click on **`Add new rule`**
- Then set `Rule Number` as **`110`** and `Type` must be **`HTTPS(443)`** and then click on **`Add new rule`** set `Rule Number` as **`120`** and `Type` must be **`Custom ICMP - IPv4`** and click on Save changes
- And now we are good to go for `Subnet associations` and click on **`Edit subnet association`** and select `private subnets` and click on **`Save changes`**

# <h3>Creating Security Groups:
-  Head to **`Security groups`** under `Security` dropdown and click on **`Create security group`** on the top right of the page 
-  Set a security group name for `public` and choose the selected VPC and then click on **`Add rule`** for `Inbound rules` and update `Port range` with **`22`** and `Source` with **`My IP`** then click on **`Create security group`**
-  And we must also create an another security group for `private` and click on **`Create security group`** and set a security group name for `private` and choose the selected VPC and then click on **`Add rule`** for `Inbound rules` and update `Port range` with **`22`** and `Source` with **`192.168.0.0/24`** then click on **`Create security group`**
-  Then head to Outbound rules and click on **`Edit outbound rules`** and click on `Add rule` and select **`HTTPS`** and **`All ICMP - IPv4`** for `Type` and **`0.0.0.0/0`** for `Destination` and then click on **`Save rules`**
- The security group will be used for `Network Setting in EC2`

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

# <h3>Testing Connectivity:
- Select the created instance and copy the **`Public IPv4 address`**
- Open the terminal and run `ssh -i ~/Downloads/bashion.pem ec2-user@<the copied public IPv4 address>`
- If you have permission issue due to read and write, please run `chmod 400 ~/Downloads/bastion.pem` to update your permission with **_`READ ONLY`_** then run `ssh -i ~/Downloads/bashion.pem ec2-user@<the copied public IPv4 address>` again to verify the connectivity!
- In addition, you may also run `whoami` to verify **`ec2-user`**