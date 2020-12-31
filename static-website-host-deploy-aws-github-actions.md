# Overview
S3 is the obvious place to host a static (frontend code only) website on AWS. It's a simple, serverless way to store and serve files without running a server or fiddling with a storage server, scales effortlessly, and is very inexpensive, with a free tier and pay per request modal. 

In theory, all you have to do is dump some files in a S3 bucket, set permissions on the bucket to allow public access and static site hosting, and forward your domain to it with a CNAME DNS value. In practice however, this approach has two issues: S3 buckets by themselves don't support HTTPS, and you need to upload files manually to S3. This article goes over a slightly more advanced setup with CloudFront for caching and HTTPS, and GitHub Actions for CI/CD. 

There are much easier free or virtually free options for hosting static sites such as GitHub pages, but if you want control over your infrastructure, a production website, or a bit of AWS experience to show off, this is a great way to go. 

# Assumptions
This article assumes that you're already setup on AWS, have a domain or subdomain you want to use, and have code in GitHub. 

## S3 Bucket
The files will be stored in a S3 bucket. The name doesn't really matter, but you need to enable static website hosting on the bucket and allow public read access to it. 

First, go to the Properties tab on the S3 bucket's page, and enable static web hosting. Take note of the bucket's website URL. Go to the Permissions tab and click edit under "Block public access (bucket settings)". Untick all the checkboxes and save the changes. Add the following policy to the bucket policy.

```json
{
    "Version": "2012-10-17",
    "Id": "Policy1589309574299",
    "Statement": [
        {
            "Sid": "Stmt1589309569196",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "REPLACE_WITH_BUCKET_ARN/*"
        }
    ]
}
```

## HTTPS Certificate
Create a HTTPS certificate for your domain or subdomain in the AWS Certificate Manager. You must use the North Virginia AWS region for this certificate to be seen by CloudFront, no matter what region you set your CloudFront distribution up in. If you don't have your domain in AWS Route 53, you'll need to verify that you own the domain/subdomain by setting some DNS records on it.

## CloudFront
You also need to create a CloudFront web distribution. Most of the settings don't really matter for this to work, here are the ones that do:
* Origin Domain Name - CloudFront provides a handy dropdown list, but this fills the field in with the S3 bucket's API URL, which works but doesn't provide automatic redirects from a folder to index.html.
* Origin Path - Leave blank if you want to use all files in the bucket. Asterisks don't work - they're taken literally.
* Alternate Domain Names (CNAMEs) - List the domain names that the distribution will be accessed by
* SSL Certificate - Choose a custom SSL certificate. This choice only becomes active after CloudFront detects a SSL cert in CM in the correct region. Takes some time after it's done to actually register it.

## DNS
Forward your domain/subdomain to the CloudFront distribution's URL (*.cloudfront.net) with a CNAME DNS entry. If you're not using Route 53, you won't be able to forward the root domain to CloudFront out of the box, but there are a few free services that'll do it for you.

## Github Actions
Most of the work here is already done - there's a couple of excellent pre baked actions. I find that <a href="https://github.com/reggionick/s3-deploy">reggionick/s3-deploy</a> works the best for this scenario. You simply need to use the example action, add, change or remove the build step, create the needed repository secrets and add the workflow to your repo.