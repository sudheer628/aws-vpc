If you enable "DNS Resolution" but disable "DNS Hostnames" while creating a VPC in AWS, the following outcomes will occur:

### Outcomes

1. **Internal DNS Resolution:**
   - Instances within the VPC will still be able to resolve domain names to IP addresses using the Amazon-provided DNS server. This means they can look up and connect to both internal and external resources by their DNS names.

2. **No Public DNS Hostnames:**
   - Instances that you launch in the VPC will not receive public DNS hostnames, even if they have public IP addresses. This means that while the instances can resolve and reach external domain names, they themselves cannot be easily reached via a public DNS name.

3. **Private DNS Names:**
   - Instances will not have private DNS hostnames either. They will only have their private IP addresses for internal communication. This makes managing internal communication slightly more complex as you will need to use IP addresses instead of hostnames.

4. **Access to AWS Services:**
   - Instances can still access AWS services like Amazon S3, DynamoDB, etc., because they can resolve the DNS names of these services.

5. **Hybrid Cloud and On-Premises Integration:**
   - Your VPC can still resolve domain names for resources in a hybrid environment. However, the instances in the VPC will not have DNS names, making it harder to refer to them from external systems.

### Example Scenario

Consider an application with multiple instances in the VPC that needs to connect to external APIs and AWS services, but does not need to be accessible via DNS names. In this case, enabling DNS resolution ensures that the instances can resolve and connect to external domains and AWS services.

However, if you later decide to create an EC2 instance with a public IP for web hosting or some other service that needs to be accessed via a DNS name, you won't be able to use a public DNS hostname to refer to this instance. You'd have to use the public IP address directly or set up a custom domain with a corresponding DNS record.

### Configuring Route 53 Private Hosted Zones

Even with DNS hostnames disabled, you can still use Amazon Route 53 private hosted zones for internal DNS management. This allows you to create custom domain names for your instances, but these names won't be automatically assigned by AWS.

### Summary

- **DNS Resolution Enabled, DNS Hostnames Disabled:** Instances can resolve external and internal domain names but will not have DNS hostnames themselves.
- **Implications:** Instances won't be easily accessible via DNS names, potentially complicating internal and external communication.
- **Use Cases:** Suitable for environments where instances need to connect to external services but don't need to be accessed via DNS names. 

This configuration can work for many internal applications but may require additional setup if DNS hostnames for instances become necessary later.
