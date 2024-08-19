

# S3 Static Website Hosting with Route 53, CloudFront, and AWS Certificate Manager

This README provides a step-by-step guide to hosting a static website on AWS using S3, Route 53, CloudFront, and AWS Certificate Manager (ACM) for HTTPS.

## Prerequisites

- AWS Account: Ensure you have an active AWS account.
- Domain Name: You need a registered domain name, which can be managed through AWS Route 53 or any other DNS provider.
- AWS CLI: Install the AWS Command Line Interface (CLI) for managing your AWS resources from the command line.

## Step 1: Set Up the S3 Bucket

1. Create an S3 Bucket:
   - Navigate to the S3 service in the AWS Management Console.
   - Create a new bucket and name it exactly as your domain name (e.g., `example.com`).
   - Enable **static website hosting** under the bucket's properties.

2. Upload Your Website Files:
   - Upload your static website files (HTML, CSS, JavaScript) to the bucket.
   - Set the bucket policy to allow public read access:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Sid": "PublicReadGetObject",
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:GetObject",
           "Resource": "arn:aws:s3:::example.com/*"
         }
       ]
     }
     ```

3. Test the Website:
   - Access the website via the S3 bucket URL (e.g., `http://example.com.s3-website-us-east-1.amazonaws.com`).

## Step 2: Set Up Route 53 (DNS Management)

1. Create a Hosted Zone:
   - In Route 53, create a hosted zone for your domain (e.g., `example.com`).

2. Configure DNS Records:
   - Add an **A Record** or **Alias Record** pointing to your S3 bucket.

## Step 3: Set Up AWS Certificate Manager (ACM)

1. Request a Certificate:
   - Navigate to ACM and request a public certificate.
   - Select DNS validation.
   - Enter your domain name (e.g., `example.com` and `www.example.com`).

2. Validate the Domain:
   - ACM will provide a DNS record that you need to add to Route 53.
   - After adding the DNS record, ACM will validate the domain and issue the certificate.

## Step 4: Set Up CloudFront (Content Delivery Network)

1. Create a CloudFront Distribution:
   - Navigate to the CloudFront service and create a new distribution.
   - Set the origin to your S3 bucket (e.g., `example.com.s3.amazonaws.com`).

2. Configure SSL/TLS:
   - Under the SSL/TLS settings, select the certificate you issued in ACM.
   - Enable **HTTPS** as the default protocol.

3. Configure Caching and Behaviors:
   - Set up caching and behaviors according to your needs (e.g., TTL, query strings).

4. Update Route 53 Records:
   - Add an **A Record** or **Alias Record** in Route 53 pointing to the CloudFront distribution.

## Step 5: Test and Deploy

1. Access Your Website:
   - Your website should now be accessible via HTTPS using your domain name (e.g., `https://example.com`).

2. Monitor and Maintain:
   - Use CloudFront and S3 metrics to monitor the performance and access patterns.
   - Regularly update content in the S3 bucket and invalidate CloudFront cache as needed.

## Conclusion

You have successfully set up a static website on AWS using S3, Route 53, CloudFront, and AWS Certificate Manager. This setup ensures your website is secure, scalable, and performant with global CDN distribution.

