https://cloud.google.com/appengine/docs/flexible/configuration-files


#App Engine Directory Structure
![image](https://github.com/user-attachments/assets/ca883b01-04d7-4149-bcb1-273098c39ac6)
app.yaml to be added in the same location as the app's source files

#WordPress file structure
https://learn.wordpress.org/lesson/the-wordpress-file-structure/

#App Engine Flexible Deployment

Step 1: Create Cloud SQL instance with Private Service Connect enabled

Method1 - https://cloud.google.com/sql/docs/mysql/configure-private-service-connect#create-cloud-sql-instance-psc-enabled-2

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

Method 2 - https://cloud.google.com/sql/docs/mysql/connect-instance-private-ip#create-instance
