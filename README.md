# PostGIS live database of NYC for deployment and querrying
A geospatial database of New York City using AWS, Docker, PostgreSQL, PostGIS and PgAdmin

## Setting up AWS

### Creating an EC2 instance

1. Create a AWS Free Tier account - more on how to [here](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc).
2. In AWS Management Console, go to *All services* -> *EC2*. Then go to the *Launch instance* box and click on *Launch instance*.
3. Then, choose *Amazon Linux 2 AMI* (this name could vary slightly between regions but should always include *Amazon* and *Linux*) and choose either x86 or Arm - I chose *x86*.
3. Choose the Free tier elegible option - in my case was *t2.micro* and then click *Next: Configure Instance Details*.
4. In here, leave everything as defaul but create an IAM role. Find the *IAM role* option and click on *'Create new IAM role'*.
5. In the new window, click on *'Create role'*, select *AWS service* and choose as a use case *EC2*.
6. In the create role window, type *'ssm'*, tick *AmazonEC2RoleforSSM*, click on *Next: Tags* and create a tag where *Key* = 'Name' and *Value* = 'theNameOfYourInstance'. Then click *Next: Review*.
7. In the create role window, give role a name i.e., SSMpostgis and click *Create role*.
8. Go back to the window from the step 5 and the *IAM role* option select the role you just created.
9. Skip storage and tag rections by accepting the default options.
10. In the Configure Security Group page, add a new rule and select *HTTPS*, click *Review and Launch* and then *Launch*.
11. Lastly, make sure you create a new key pair for your private key. Select *Create a new key pair*, give it a name and download the key pair. Make sure you keep this file in a safe place (you'll use it in this tutorial).
12. Voila. You have successfully created an AWS EC2 instance. Go to the main menu and into EC2 and make sure you run your instance.

### Setting up instance for deployment

1. Go to the main menu and type on the search bar on the top and type 'Systems Manager' and select *Systems Manager*.
2. In the right hand side bar, click on *Session Manager* -> *Start session* and the new instance will appear. Select it and click *Start session*. Then a new terminal screen will appear.
3. Log as a super user
```bash
sudo su
```
4. Install docker
```bash
yum install docker -y
```
5. Install docker-compose, give permissions and create a symbolic link
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
```bash
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
6. Start docker
```bash
service docker start
```
7. Install git
```bash
yum install git -y
```
8. Git clone the docker-composer.yml
```bash
git clone https://github.com/jjcfrank/docker-compose-postgis.git
```
9. Go into the cloned folder
```bash
cd docker-compose-postgis
```
10. Run the docker container
```bas
docker-compose up
```
