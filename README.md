# AWS Advanced Developer Course
## Day 1: Static S3 Website Hosting

1. For ease of CLI commands set 

    `mybucket=notes-bucket-jdencker-60614`
2. update the public-access-block permissions on the bucket

    `aws s3api put-public-access-block --bucket $mybucket --public-access-block-configuration "BlockPublicPolicy=false,RestrictPublicBuckets=false"`
3. sync the html files in this repository with the bucket

    `aws s3 sync ~/environment/labRepo/html/. s3://$mybucket/`
4. enable Amazon S3 website hosting

    `aws s3api put-bucket-website --bucket $mybucket --website-configuration file://~/environment/labRepo/website.json` 
    
    Note: `website.json` specifies which objects to use fo the index and error documents

5. Update `policy.json` to reflect your bucket name

    Under `Statement:` -> `"Resource":arn:aws:s3:::notes-bucket-jdencker-60614`

6. Apply bucket policy

    `aws s3api put-bucket-policy --bucket $mybucket --policy file://~/environment/labRepo/policy.json`

7. Pull bucket region out from AWS API

    `region=$(curl http://169.254.169.254/latest/meta-data/placement/region -s)`

8. From this address https://docs.aws.amazon.com/general/latest/gr/s3.html#s3_website_region_endpoints choose the approriate endpoint for the region

    base_url = \<insert_url_from_website\>

9. Access website from: http://$mybucket.$base_url
