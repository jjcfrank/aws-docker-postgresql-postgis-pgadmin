# A PostGIS database of NYC in AWS
Creating a geospatial database of New York City using AWS, Docker, PostgreSQL, PostGIS and PgAdmin

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
docker-compose up -d
```
11. Leave it running and exit the window

### Viewing the docker

1. Go back to the AWS main menu and then into the EC2 section.
2. On the *Resources* box, click on the *Instances (running)* which it should have at 1 instance running.
3. When you spot the instance, click on the *Instance ID* which will open a summary of the instance.
4. You container's public ip address should be under *'Public IPv4 address'*.
5. Copy and paste the public ip address into your browser.
6. Voila. You have successfully installed PostgreSQL with PostGIS which is available via PgAdmin.

### Default credentials

Email Address / Username = admin@admin.com
Password = root

**You can change this and other credentials by editing the docker-compose.yml file that is inside the EC2 instance**

### Setting up and accessing the docker database

1. Log into PgAdmin with the credentials from above.
2. Right click on *Servers* -> *Create* -> *Server*.
3. In the *General* tab choose a preferred name i.e., 'myDB'.
4.1 In the *Connection* tab type the ip address (note this is NOT the public ip address for the instance but the ip address of the docker container). To find the ip address of the docker container follow steps 1 to 3 from *'Setting up instance for deployment'* and then type
```bash
docker ps
```
4.2 A list of running containers will display - copy the *CONTAINER ID* for the database which you can identify under *NAMES*. The container ends with *'...db_1'*.
4.3. Find the address by typing
```bash
docker inspect theContainerIDhere | grep IPAddress
```
5. Leave everything default but change user name as *docker* and password *docker*.
6. Voila. The database should have connected.




## License & copyright

© Frank Jimenez

Licensed under the [MIT Licence](LICENSE).


