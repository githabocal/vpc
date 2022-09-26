# VPC - Setup and Test Connection

First step to do is creating **`VPC`** then apply the components of VPC that are as follows;
- *Subnets*
- *Internet Gateways (igw)*
- *Route Tables*
- *NAT Gateways*
- *Network ACLs*
  
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

# <h3>Creating Internet Gateway(igw):
- Head to **`Internet gateways`** under `Virtual private cloud` dropdown and click on **`Create internet gateway`** on the top right of the page
- Then set a name for internet gateway and click on **`Create internet gateway`**
- After creating a internet gateway, we must attach it to the **`created VPC`** and thus click on **Actions** dropdown and choose **`Attach to VPC`** then select the **`created VPC`** and click on **`Attach internet gateway`**
- Then once you head to internet gateways, you must be able to verify that **`State is Attached`**