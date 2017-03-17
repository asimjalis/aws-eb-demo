# Elastic Beanstalk Demo

Download code.

    git clone https://github.com/awslabs/eb-tomcat-snakes
    cd eb-tomcat-snakes

Build it.

    ./build.sh

Install command line Elastic Beanstalk tool.

    sudo pip install awsebcli

If you have already installed the tool use this command to update it.

    sudo pip install --upgrade awsebcli

Initialize project

    eb init

Choices to make in the `eb init` command line dialog.

    Select a default region
    us-west-2

    Select an application to use
    Create new Application

    Enter Application Name
    eb-tomcat-snakes

    Select a platform.
    Java

    Select a platform version.
    Tomcat 8 Java 8

    Do you wish to continue with CodeCommit? (y/n) (default is n): 
    n

    Do you want to set up SSH for your instances?
    y

    Select a keypair.
    Create new KeyPair

Create environment.

    eb create tomcat-snakes-0 \
      --sample \
      --single \
      --timeout 20 -i t2.micro \
      --database.engine postgres \
      --database.instance db.t2.micro \
      --database.username demouser \
      --database.password demopassword

This will take a few minutes.

Get the RDS host name from the log files.

    eb logs | grep RDS_HOSTNAME

Save it into a local variable `db_host`.

This is what it will look like.

    db_host=aa11m3x9tqrvme1.cuyh34rlvfmp.us-west-2.rds.amazonaws.com

Deploy environment.

    eb deploy --staged

Open in browser. 

    eb open

Connect to the instance.

    eb ssh

Install PostgreSQL client.

    sudo yum -y install postgresql94

Init `db_host` on EC2 instance using same value as before.

    db_host=aa11m3x9tqrvme1.cuyh34rlvfmp.us-west-2.rds.amazonaws.com

Connect using host name from output of create environment step.

    psql \
      --dbname=ebdb \
      --host=$db_host \
      --username=demouser 

Run queries.

    SELECT COUNT(*) FROM movies;

    SELECT * FROM movies LIMIT 10;

Quit PostgreSQL client shell.

    \q

Exit ssh.

    exit

View `build.sh`.

    less ./build.sh

View config file.

    less .elasticbeanstalk/config.yml 
