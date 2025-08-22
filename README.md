# Static_Site_in_S3
Scenario 1: Allow access to all **(If, Block public access: OFF)**
<br>
**Note:** If, **Block all public access: On**, below script wont work

{

    "Version": "2012-10-17",
    
    "Statement": [

       {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::awsdemo-site1/*"
        }
    ]
}


Scenario 2: If, **Block all public access: On**
host a static site in s3 with ip based access control

{

        "Version": "2012-10-17",

        "Id": "IPAllow",
        
        "Statement": [
        
            {
                "Sid": "AllowIPAccess",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::your-bucket-name/*",
                "Condition": {
                    "IpAddress": {
                        "aws:SourceIp": ["XX.XX.XX.XX/32", "YY.YY.YY.YY/24"]
                    }
                }
            },
            {
                "Sid": "DenyAllExceptIPs",
                "Effect": "Deny",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::your-bucket-name/*",
                "Condition": {
                    "NotIpAddress": {
                        "aws:SourceIp": ["XX.XX.XX.XX/32", "YY.YY.YY.YY/24"]
                    }
                }
            }
        ]
    }
