# Setting Up DNS in Your VPC<a name="set-up-vpc-dns"></a>

Domain Name System \(DNS\) is a standard by which names used on the internet are resolved to their corresponding IP addresses\. A DNS hostname uniquely names a computer and consists of a host name and a domain name\. DNS servers resolve DNS hostnames to their corresponding IP addresses\.

To set up DNS in your VPC, ensure that DNS hostnames and DNS resolution are both enabled in your VPC\. The VPC network attributes `enableDnsHostnames` and `enableDnsSupport` must be set to `true`\. To view and modify these attributes, go to the VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\. 

For more information, see [Using DNS with your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\. 

**Note**  
If you are using Route 53, confirm that your configuration does not override DNS network attributes\.