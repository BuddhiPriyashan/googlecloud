https://cloud.google.com/appengine/docs/flexible/configuration-files


#App Engine Directory Structure
![image](https://github.com/user-attachments/assets/ca883b01-04d7-4149-bcb1-273098c39ac6)
app.yaml to be added in the same location as the app's source files

#WordPress file structure
https://learn.wordpress.org/lesson/the-wordpress-file-structure/

#App Engine Flexible Deployment

Step 1: Create Cloud SQL instance with Private Service Connect enabled

Method1 - 
https://cloud.google.com/sql/docs/mysql/configure-private-service-connect#create-cloud-sql-instance-psc-enabled-2

step 1 - Create a Cloud SQL instance with Private Service Connect enabled
gcloud sql instances create bdy-sql-wordpress \
--project=www-wordpress-website \
--region=europe-west3 \
--enable-private-service-connect \
--allowed-psc-projects=www-wordpress-website \
--availability-type=ZONAL \
--no-assign-ip \
--tier=db-custom-1-3840 \
--database-version=MYSQL_8_0 \
--enable-bin-log


step 2 - Get the service attachment URI (pscServiceAttachmentLink)
gcloud sql instances describe bdy-sql-wordpress --project=www-wordpress-website

pscServiceAttachmentLink: projects/t3d25f70f25d9d065p-tp/regions/europe-west3/serviceAttachments/a-68cacb6bf974-psc-service-attachment-bd688f4a8492e54e


step 3 - Create a Private Service Connect endpoint
gcloud compute addresses create psc-prod-ip \
--project=www-wordpress-website \
--region=europe-west3 \
--subnet=subnet1-bdy \
--addresses=10.156.1.10

step 4: Create the Private Service Connect endpoint and point it to the Cloud SQL service attachment

gcloud compute forwarding-rules create bdy-psc-endpoint-sql \
--address=psc-prod-ip \
--project=www-wordpress-website \
--region=europe-west3 \
--network=default-bdy \
--target-service-attachment=projects/t3d25f70f25d9d065p-tp/regions/europe-west3/serviceAttachments/a-68cacb6bf974-psc-service-attachment-bd688f4a8492e54e \
--allow-psc-global-access

step 5: Connect to a Cloud SQL instance
You can connect to a Cloud SQL instance with Private Service Connect enabled by using an 
 a) internal IP address, 
 b) a DNS record, 
 c) Cloud SQL Auth Proxy, 
 d) Cloud SQL Language Connectors, 
 e) other Google Cloud applications


gcloud sql instances create INSTANCE_NAME \
--project=PROJECT_ID \
--region=REGION_NAME \
--enable-private-service-connect \
--allowed-psc-projects=ALLOWED_PROJECTS \
--availability-type=AVAILABILITY_TYPE \
--no-assign-ip \
--tier=MACHINE_TYPE \
--database-version=DATABASE_VERSION \
--psc-auto-connections=network=VPC_NETWORK,project=SERVICE_PROJECT \
--enable-bin-log

step 6: find DNS name of the SQL instance:
gcloud sql instances describe bdy-sql-wordpress --project=www-wordpress-website | grep dns
dnsName: 68cacb6bf974.1g9aeq62dt1al.europe-west3.sql.goog.

step 7: create private DNS zone (neend to have enabled DNS api: dns.googleapis.com)
gcloud dns managed-zones create dns-zone-for-sql \
--project=www-wordpress-website \
--description=bdy-dns-zone \
--dns-name=68cacb6bf974.1g9aeq62dt1al.europe-west3.sql.goog. \
--networks=default-bdy \
--visibility=private

step 8: create a DNS record in the zone
gcloud dns record-sets create 68cacb6bf974.1g9aeq62dt1al.europe-west3.sql.goog. \
--project=www-wordpress-website \
--type=A \
--rrdatas=10.156.1.10 \
--zone=dns-zone-for-sql

#Create App engine flexible in the same VPC
Obs: In order to connect to your Cloud SQL instance over private IP, your App Engine flexible environment deployment must be in the same VPC network as your Cloud SQL instance.

$ sudo apt update
$ sudo apt install php-zip php-curl
$ composer install

php wordpress.php setup -n \
    --dir=./wordpress-project \
    --db_instance=bdy-sql-wordpress \
    --db_name=wordpress-database-bdy \
    --db_user=www-wordpress-website@appspot.gserviceaccount.com \
    --project_id=www-wordpress-website \
    --db-region=eu-west3 \
    --db_password=password









Method 2 - https://cloud.google.com/sql/docs/mysql/connect-instance-private-ip#create-instance


Method 3 - https://cloud.google.com/sql/docs/mysql/connect-instance-private-ip

Download the Cloud SQL Auth Proxy:
wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy

Make the Cloud SQL Auth Proxy executable:
chmod +x cloud_sql_proxy

Start the Cloud SQL Auth Proxy:
./cloud_sql_proxy -instances=<project_id>:<region>:<sql_instance_name>=tcp:3306

In another shell session, connect to SQL database using the mysql client:
mysql -u root -p --host 10.115.48.3 --port 3306
24/12/22 10:56:40 couldn't connect to "wordpress-ro-15dec24:us-central1:quickstart-mysql-instance": dial tcp 10.115.48.3:3307: connect: connection timed out


#### Firewall Rules
![{FDCACFAB-5DC4-46F0-996D-0AB6D937AA78}](https://github.com/user-attachments/assets/add928f2-ded9-4505-94c6-4713f7a5264f)

![{D389D0A6-E640-441A-98BD-6647BD38DE2A}](https://github.com/user-attachments/assets/f27af36a-5663-4a64-9844-ed645b702961)

![{83C9CC99-2358-4E30-B9BB-FFBAB3CBA7EF}](https://github.com/user-attachments/assets/7bd33c8e-b6bd-4bc2-9dcc-b641c067dcfa)

![{8A184926-F4CC-4794-B8FE-6AC6A093B4F3}](https://github.com/user-attachments/assets/f0bb1e88-aa58-4277-9c55-212d8ddbb8a6)

student-00-0a3ae24a3983@wordpress-private-ip:/var/www/html$ ls
index.php        wp-admin              wp-content         wp-load.php      wp-signup.php
license.txt      wp-blog-header.php    wp-cron.php        wp-login.php     wp-trackback.php
readme.html      wp-comments-post.php  wp-includes        wp-mail.php      xmlrpc.php
wp-activate.php  wp-config-sample.php  wp-links-opml.php  wp-settings.php

