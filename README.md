# S3-Static-Website
Deploy a static website in AWS using S3, Route 53, CloudFront, certificate manager and pipelines w/ githib.

1. In your AWS console go to S3 bucket.

2. In you S3 bucket splash screen, create a new bucket and name it after the domain name you are wanting to use. (ex. johndoe.com)

3. Configure the S3 bucket appropriatly. Ensure that public access to the bucket is set to no access. Lots of other users will suggest that under properties, bottom of the page, section called "Static Website Hosting" should be enabled... dont enable this, leave it disabled. (This will cause confusion between S3 and cloudfront w/ a certificate when enabling https)

4. Route 53 gives you the ability to purchase and manage your domain names.  Ensure that you have purchased a valid domain name in Route 53 and we will come back to this.

5. In certificate manager, request a certificate for your recently purchased domain name. (make sure the region location is going to be the same as cloudfront. ex Oregon west 2) Be sure when setting up the the certifacte to include *.johndoe.com. This will allow whatever subdomain you might choose to use to be secure as well. Once created, add the certificate to your domain in route 53. At this time in Route 53, create a new CNAME record with www.johndoe.com pointing toward johndoe.com

6. In pipelines, create a new pipeline that links your github account to your amazon S3 bucket.  This will allow for a CI/CD code to be updated automaticly when pushed to github from your local machine.

7. In cloudfront, create a new distribution. In Origin domain choose your amazon s3 bucket. Under Origin access make sure to select "Origin access control settings". This will only allow your S3 bucket to restrict access to only cloudfront. In origin access control select your S3 bucket where your are hosting your website documents. Also, a bucket policy giving cloudfront access to your s3 bucket will be created when finished. Under viewer protocol policy select the "Redirect http to https" radio button. Web Application Firewall (WAF) select "Do not enable security protection".  Under Alternate domain name be sure to add "johndoe.com" and "www.johndoe.com". Custom SSL certificate is where you will select the certifacte you created in step 5.  Be sure to list the "Default root object" to index.html. Hit "Create distribution" at the bottom of the screen.

8. Congrats, you should be hosting your website.

9. When pushing new code through your pipeline you might not see the most up-to-date code on your web page. In order to fix this you must, after pipeline is finished deploying, is to go to cloudfront. Click on your distrubution and navigate to "Invalidations". Create a "New Validation", under object path type in "*/", and click "Create Validation". This will insure that cloudfront is using the most up-to-date files in your S3 bucket.

