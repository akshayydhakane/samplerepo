# Host Static Webiste in S3 bucket Using CloudFront

## **S3 Bucket**

### Create S3 Bucket

1. Login to AWS account and go to S3  

2. Select the create bucket button for the create new bucket.
<!--image-->  
![s3](./image/create_s3_bucket_.jpg)
3. Enter the bucket name and select the region then click on NEXT button.  
<!--image-->  
![s3](./image/bucketname.jpg)  
4. select block all public access  
<!--image-->
![s3](./image/bucket_policy.jpg)

---

## **IAM USER**

1. create new user
2. add user name and select authentication type  
  = if you click on access key then you will received a access and secret key after create a user
<!--image-->
![iam](./image/iam_user.jpg)
3. create policy for iam user to access s3 bucket if you have give any other access then you have to search policy name either you have to create new policy for related access.  
<!--image-->
![iam](./image/create_iam_user_poilicy.jpg)
4. then after add tag for your reference
<!--image-->  
![iam](./image/iam_user_tag.jpg)
5. then review your new iam user details and clck create user
<!--image-->
![iam](./image/review_user.jpg)
6. at last you'll receive a access and secret key for access iam user  
<!--image-->
![iam](./image/iam_user_created.jpg)

---

## **Cloud Front Distribution**

Click the Create Distribution button for the create.  

1. Select the origin Domain Name : select your s3 bucket

2. origin path automatically generated if you select any s3 bucket  

3. Now,select origin acces = Legacy access identities and click on create new OAI

4. select Bucket policy = Yes,update the bucket policy  
<!--image-->
![CDN](./image/distribution_step.jpg)
5. Select the viewer protocol policy: select the Redirect HTTP to HTTPS.
<!--image-->
![CDN](./image/http_to_https.jpg)

---

## S3 Bucket

Then enter in bucket. Then select the PERMISSION option.  

Enter in the BUCKET POLICY option.  

Enter the policy of the bucket for the PUBLIC access and then SAVE it.  

```bash
POLICY Example:
```

```json
{
    "Version": "2012-10-17",
    "Id": "Policy1600333364768",
    "Statement": [
        {
            "Sid": "Stmt1600333363057",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::learningbucketfordocument/*"
        },
        {
            "Sid": "2",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity E1OS17P5EDYQL7"                                                            ---change the cloudfront distribution ID
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::learningbucketfordocument/*"     ---change bucket ARN
        }
    ]
}
```
