Create a bucket named exactly same as domain_name purchased
reference : https://www.youtube.com/watch?v=V2WOgeuKdOw

(Optional)
Create a second bucket for the sub-domain (e.g., www.example.com) 
if you want to redirect www traffic to your main root domain.

1. ibsindia.site
2. www.ibsindia.site

set properties with bucket policy with followed JSON
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

