# VPC - Setup and Test Connection

Components of VPC are as follows;
- *Subnets*
- *Internet Gateways*
- *Route Tables*
- *NAT Gateways*
- *Network ACLs*
  
# <h3>Creating VPC:
- Head to Your VPCs in AWS and choose **`Create VPC`** on the top right of the page
- **`VPC only`** must be selected for **`resources to create`**
- Set a name for VPC then enter **`IPv4 CIDR`** as **`192.168.0.0/24`**. We prefer **`/24`** as **_CIDR_** because it is more secure and number of IPs limited
- **`Tenancy`** must be selected as **`Default`**; `NOT Dedicated`
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
- 2(two) public and 2(two) private subnets have been created successfully