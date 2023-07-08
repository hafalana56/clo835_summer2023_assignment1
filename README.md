# Install the required MySQL package

sudo apt-get update -y
sudo apt-get install mysql-client -y

# Running application locally
pip3 install -r requirements.txt
sudo python3 app.py
# Building and running 2 tier web application locally
### Building mysql docker image 
```docker build -t my_db -f Dockerfile_mysql . ```

### Building application docker image 
```docker build -t my_app -f Dockerfile . ```

### Running mysql
```docker run -d -e MYSQL_ROOT_PASSWORD=pw  my_db```


### Get the IP of the database and export it as DBHOST variable
```docker inspect <container_id>```


### Example when running DB runs as a docker container and app is running locally
```
export DBHOST=127.0.0.1
export DBPORT=3307
```
### Example when running DB runs as a docker container and app is running locally
```
export DBHOST=172.17.0.2
export DBPORT=3306
```
```
export DBUSER=root
export DATABASE=employees
export DBPWD=pw
export APP_COLOR=blue
```
### Run the application, make sure it is visible in the browser
```docker run -p 8080:8080  -e DBHOST=$DBHOST -e DBPORT=$DBPORT -e  DBUSER=$DBUSER -e DBPWD=$DBPWD  my_app```

### The second part of the assignment is to deploy the application using pods
first from the document folder, switch to /tfcode
run "terrafrom init"
then "terraform validate"
then "terraform apply --auto-approve"
to create the ec2 instance that would act as the host for the application

### After creating the ec2 instance:
-copy the manifests folder into the tmp dir of the ec2 instance
-run  the init_kind.sh file to install all the neccessary packages in the ec2 instance
-There may be a permission error when trying to run the init_kind.sh file, we just 
-need to give the file permission by running the command "chmod 777 init_kind.sh"
-After installing all the necessary packages we need to pull the images of both the mysql and web application from ECR
-From there we sitch to /mysql folder to create mysl pod, deployment and replica sets
-Then we switch /webbapp and we create the application pod, deployment and replica sets. Then we run the clusterIP service to expose the
application internally:
we run "curl localhost:8080"
-Run the service manifest to expose the application externally. We need to expose port 30000 as stated in the assignment
from the host ec2 instance security group
-We should be able to access the application on the browser wit the IP address of the host ec2 instance together with the port that IP:30000
