To host a static website on Amazon S3 using a custom domain registered with GoDaddy, you must route your DNS traffic through AWS Route 53. This is because Amazon S3 endpoints rely on dynamic IP addresses, and GoDaddy’s native DNS does not support pointing a root domain (e.g., example.com) directly to an AWS alias endpoint.

### Create a bucket named exactly same as domain_name purchased
reference : https://www.youtube.com/watch?v=V2WOgeuKdOw

(Optional)
### Create a second bucket for the sub-domain (e.g., www.example.com)
if you want to redirect www traffic to your main root domain.

1. ibsindia.site 
2. www.ibsindia.site

### Enable Static Website Hosting in Properties tab & update bucket policy with followed JSON
(Note: Turn off Block all public access, in Permissions tab)
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

### Create a Hosted Zone in Route 53
for that need to set up a DNS zone inside AWS to manage GoDaddy domain records.
1. In the AWS Console, navigate to Route 53.
2. Click Hosted zones in the left sidebar and select Create hosted zone.
3. Enter your exact domain name (e.g., example.com), make sure Public hosted zone is selected, and click Create.
4. Route 53 will automatically generate four Name Server (NS) records. Keep this tab open—you will need these addresses next.

### Update Nameservers in GoDaddy
You must hand over DNS management authority from GoDaddy over to AWS Route 53.
1. Log into your GoDaddy Control Panel.
2. Navigate to your Domain Portfolio and select the domain you are connecting.
3. Click on DNS or Manage DNS, then look for the Nameservers section.
4. Click Change Nameservers (or I'll use my own nameservers).
5. Paste the four AWS NS addresses you generated in Route 53 into the fields.
	• Note: Remove the trailing period/dot at the end of each AWS server string when pasting them into GoDaddy.
6. Save your changes.
(⚠️ Propagation Notice: DNS changes can take anywhere from a few minutes to 24–48 hours to fully propagate across the global internet.)

### Point Route 53 to Your S3 Bucket
Now that AWS handles your domain's traffic, point the domain directly to your static S3 site.
1. Return to your Route 53 Hosted Zone for your domain.
2. Click Create record.
3. Leave the Record name blank to configure the root/naked domain (example.com).
4. Select A — Routes traffic to an IPv4 address and some AWS resources as the record type.
5. Toggle the Alias switch to enabled.
6. In the Route traffic to dropdown menus, select:
	• Alias to S3 website endpoint
	• Choose the AWS Region where you created your S3 bucket (e.g., us-east-1)
	• Select your S3 bucket target from the list
7. Click Create records

### Next Step Recommendation (HTTPS Support)
By default, mapping an S3 bucket directly to a custom domain over standard DNS only supports unencrypted HTTP traffic. If visitors attempt to access your site via https://, it will throw a connection failure.
