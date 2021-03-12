# PostGIS live database of NYC for deployment and querrying
A geospatial database of New York City using AWS, Docker, PostgreSQL, PostGIS and PgAdmin

## Setting up AWS

1. Create a AWS Free Tier account - more on how to [here](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc).
2. In AWS Management Console, go to *All services* -> *EC2*. Then go to the *Launch instance* box and click on *Launch instance*.
3. Then, choose *Amazon Linux 2 AMI* (this name could vary slightly between regions but should always include *Amazon* and *Linux*) and choose either x86 or Arm - I chose *x86*.
3. Choose the Free tier elegible option - in my case was *t2.micro* and then click *Next: Configure Instance Details*.
4. In here, leave everything as defaul but create an IAM role. Find the *IAM role* option and click on *'Create new IAM role'*.
5. In the new window, click on *'Create role'*, select *AWS service* and choose as a use case *EC2*.
6. In the create role window, type *'ssm'*, tick *AmazonEC2RoleforSSM*, click on *Next: Tags* and then *Next: Review*.
7. In the create role window, give role a name i.e., SSMpostgis and click *Create role*.
8. Go back to the window from the step 5 and the *IAM role* option select the role you just created.
9. Skip storage and tag rections by accepting the default options.
10. In the Configure Security Group page, add a new rule and select *HTTPS*, click *Review and Launch* and then *Launch*.
11. Lastly, make sure you create a new key pair for your private key. Select *Create a new key pair*, give it a name and download the key pair. Make sure you keep this file in a safe place (you'll use it in this tutorial).
12. Voila. You have successfully created an AWS EC2 instance.

