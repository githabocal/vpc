# VPC - Setup and Test Connection

Components of VPC are as follows;
- *Subnets*
- *Internet Gateways*
- *Route Tables*
- *NAT Gateways*
- *Network ACLs*
  
# <h3><u>Creating VPC:<h3>
- Head to Your VPCs in AWS and choose **`Create VPC`** on the top right of the page
- **`VPC only`** must be selected for **`resources to create`**
- Set a name for VPC then enter **`IPv4 CIDR`** as **`192.168.0.0/24`**. We prefer **`/24`** as CIDR because it is more secure and number of IPs limited
- **`Tenancy`** must be selected as **`Default`**; `NOT Dedicated`
- Then click on **`Create VPC`**