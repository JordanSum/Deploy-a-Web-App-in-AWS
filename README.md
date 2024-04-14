# Deploy a Web App using S3, Route 53, CloudFront, ACM and pipelines.
Deploy a static website in AWS using S3, Route 53, CloudFront, certificate manager and pipelines w/ github.

1. In your AWS console go to S3 bucket.

2. In you S3 bucket splash screen, create a new bucket and name it after the domain name you are wanting to use. (ex. johndoe.com)

![1 create bucket splash screen](https://github.com/JordanSum/S3-Static-Website/assets/144553157/b119ae0a-9353-44e3-a9b7-3b9e46ba49d2)


3. Configure the S3 bucket appropriatly. Ensure that public access to the bucket is set to no access. Lots of other users will suggest that under properties, bottom of the page, section called "Static Website Hosting" should be enabled... dont enable this, leave it disabled (This will cause confusion between S3 and cloudfront w/ a certificate when enabling https). Also, leaving this disabled will prevent any data leaks from occuring from your S3 Bucket.

![2 create bucket](https://github.com/JordanSum/S3-Static-Website/assets/144553157/8d936b0e-a240-4ee9-9f00-9c0d82cff681)

4. Route 53 gives you the ability to purchase and manage your domain names.  Ensure that you have purchased a valid domain name in Route 53, we will come back to this.

![4 create a domain](https://github.com/JordanSum/S3-Static-Website/assets/144553157/35985780-850b-477c-8dbf-ee4d76c81d3f)


5. In certificate manager, request a SSL certificate for your recently purchased domain name. (make sure the region location for ACM (AWS Certificate Manager) is going to be the same as cloudfront. ex Oregon west 2). Be sure when setting up the the certifacte to include *.johndoe.com. This will allow whatever subdomain you might choose to use to be secure as well.

![5-1 create ACM](https://github.com/JordanSum/S3-Static-Website/assets/144553157/0543c69e-3c60-46da-afcf-6ce351519eec)

![5-2 create ACM](https://github.com/JordanSum/S3-Static-Website/assets/144553157/b5bac44d-79ed-4aa4-b7df-f49f6da626f2)

![5-3 create ACM](https://github.com/JordanSum/S3-Static-Website/assets/144553157/381c77ac-5c32-4bcf-8117-35e91d7f9c56)

6. Once created, add the certificate to your domain in route 53. At this point go back to Route 53, create a new CNAME record with www.johndoe.com pointing to johndoe.com

![5-4 create ACM](https://github.com/JordanSum/S3-Static-Website/assets/144553157/0602da51-28bb-40e5-a32c-fa5e377c1a71)

![5-5 create ACM](https://github.com/JordanSum/S3-Static-Website/assets/144553157/2984df72-38bd-4a2a-9b2d-fdc698d50584)

![5-6 create ACM](https://github.com/JordanSum/S3-Static-Website/assets/144553157/1f63b37c-6191-4450-9c33-4974ecd674f5)



7. In pipelines, create a new pipeline that links your github account to your amazon S3 bucket.  This will allow for a CI/CD code to be updated automaticly when pushed to github from your local machine.

![6 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/1bef2b67-36c2-46e1-94f4-a84a15a12340)

8.  Go ahead and git your Pipeline a name and select V2 for the Pipepline type.  Select "Queued" radio button for the execution mode, this will allow your executions to be processed one by one in the order that they were recieved.  Go ahead and select a new service roll to allow you to work with Pipelines with sufficent permissions.

![6-1 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/79ccb17a-2b00-442a-87f4-d67ea6502661)

9. Connect Pipelines to your Github repository.

![6-2 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/24c01aad-76ef-4f99-8b1f-ff50405e18a6)

![6-3 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/455fc642-73dc-447e-beac-e22db7379929)

![6-4 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/cdbefacb-2751-4e50-843f-652f7ba67161)

![6-5 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/b5fbd365-b724-46c0-86ef-cc229be011c9)

10. After you have configured the source from which you are recieving your code from, go to the "Trigger" subsection and select "No filter" radio button.  Everytime updated code is pushed to your selected repository in Github, Pipelines is triggered to pull the updated code and store it into S3.

![6-6 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/b7059f46-0163-41db-827d-ef4b31dc8f4f)

11. Skip Build stage.

![6-7 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/02f3fdac-be0a-493f-b6bd-61e695e83c2a)

12.  Select where you want to deploy your code too. In this case we are selecting our Amazon S3 bucket.

![6-8 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/0d7e6f53-b1c1-4d83-a4be-c04744ed4722)

13. The Pipeline will then begin operations.

![6-9 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/71731c2d-049e-4005-9864-f2fc9f318b4e)

14. Navigate over to your S3 bucket and you will see a codepipline bucket created. Do not alter, or remove this bucket.  Click on the bucket you choose to deploy your Github code to in step 12.

![6-10 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/752075da-f4cd-4dee-a079-f553d2663a22)

15. You should find all your files from your Github Repo copied over to your S3 bucket.

![6-11 create pipeline](https://github.com/JordanSum/S3-Static-Website/assets/144553157/62cea6b6-4266-4a15-9d52-14127c281a74)

16. In Cloudfront, create a new distribution. In Origin domain choose your amazon s3 bucket. Under Origin access make sure to select "Origin access control settings". This will only allow your S3 bucket to restrict access to only cloudfront. In origin access control select your S3 bucket where your are hosting your website documents.

![7 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/149f9929-290f-4d29-baf8-63dd0fc6d599)

![7-1 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/676caaf2-5d3b-455a-a1f7-8b9a25722e53)

![7-2 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/dd9b934d-ca01-4b3e-9747-184fd3ce524c)

17. A bucket policy giving cloudfront access to your s3 bucket will be created when finished.

![7-3 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/7d5a5414-7b71-45bd-9fb0-f19492e1b95f)

![7-4 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/1eec95b8-ce9b-44c9-98d7-991d89d47289)

18. Under viewer protocol policy select the "Redirect http to https" radio button.

![7-5 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/b50c0913-cf60-4e7a-bf5f-67185bbb6cb0)

19. In the Web Application Firewall (WAF), you can go ahead and check off the "Do not enable secuirty protections" radio button.  If this is something more than a hobby project then go ahead and enable the WAF. If you choose to enble this it will accumulate charges.

![7-6 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/aba528bb-20f8-4a30-baa3-a515fd52b7e6)

20. Under Alternate domain name be sure to add "johndoe.com" and "www.johndoe.com". Custom SSL certificate is where you will select the certifacte you created in step 5.  Be sure to list the "Default root object" to index.html. Hit "Create distribution" at the bottom of the screen.

![7-7 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/f4a97da5-855e-4086-b859-0ae9314aeb32)

21. Once you have created your CloudFront Distribution a banner at the top of the screen should appear giving you the ability to "Copy Policy". Click the "Copy Policy" button, navigate back to your Amazon S3 bucket where you are keeping your files. Navigate to "Bucket Policy" under "Permissions" and place the policy here

![7-8 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/2156c4b0-7122-4bf3-a07c-18af7cfed914)

![7-9 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/53e2caf8-df8a-4202-8276-7426b778928f)

![7-10 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/ae3db529-92e3-48cb-91ac-fe5d04df89d7)

22. You can choose to navigate back to CloudFront and look at your distributions.  Once the Status is "Enabled" and the Last modified has changed to a date and time you are good to move on to the next step.

![7-11 create cloudfront](https://github.com/JordanSum/S3-Static-Website/assets/144553157/dce802d1-2154-4946-a2fa-991c50ffd3d8)

23. You are going to navigate back over to Route 53 and go to the custom domain name that you have created. Once there you are going to create a new "A" record. When creating your new record your are going to toggle the "Alias" swith and select CloudFront distribution, and the actual distro you created.  Click on Create records.

![4-2 create domain](https://github.com/JordanSum/S3-Static-Website/assets/144553157/fb8315c2-d011-4f42-83df-abfe5df38f1e)


24. Congrats, you should be hosting your website.

![8 website](https://github.com/JordanSum/S3-Static-Website/assets/144553157/3283dec6-e281-420a-8578-f77fdae18015)


25. When pushing new code through your pipeline you might not see the most up-to-date code on your web page. In order to fix this you must, after pipeline is finished deploying, go to cloudfront. Click on your distrubution and navigate to "Invalidations". Create a "New Validation", under object path type in "/*", and click "Create Validation". This will insure that cloudfront is using the most up-to-date files in your S3 bucket.

![9 post care](https://github.com/JordanSum/S3-Static-Website/assets/144553157/7897f088-7164-42b8-9b5f-06a12ee26cda)

![9-1 post care](https://github.com/JordanSum/S3-Static-Website/assets/144553157/8299885d-86fb-416c-a551-1f1f8d6c5e0c)

![9-2 post care](https://github.com/JordanSum/S3-Static-Website/assets/144553157/cfe891f8-76ba-48c0-9ff0-55fa8458f5c5)

![9-3 post care](https://github.com/JordanSum/S3-Static-Website/assets/144553157/f922631d-a8c2-4b2f-8e1d-b0ac197580f4)


