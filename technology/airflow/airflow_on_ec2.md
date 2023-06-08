---
title: How to Set Up Airflow on EC2
description: 
published: true
date: 2023-06-08T23:47:13.938Z
tags: aws, airflow, ec2, rds
editor: markdown
dateCreated: 2023-06-08T23:47:13.938Z
---

This contains instructions and scripts for setting up Airflow on an EC2 instance.

NOTE: By default, Airflow uses SQLite for a metadata database, which may be sufficient for experimental purposes but functionality is limited (e.g., no concurrency). Optional instructions are included for using an external database connection string.

## Steps

### Launch an EC2 instance
* t2.medium appears to be the minimum size as Airflow typically requires 4GB memory
* Make sure to enable Auto-assign public IP on launch
* Ensure one of your security groups allows for inbound connections on port 8080 (as well as the usual port 22 for SSH access)
* If you're using an external RDS database, you'll need to attach a security group that permits communication between the instance and the RDS. Note that AWS can set this up automatically when creating the RDS
* In Advanced details, include the following in the User Data (or run ssh into the instance and run it manually after launch):
    ```bash
    #!/bin/bash
    
    sudo apt -y update
    sudo apt -y --no-install-recommends install python3-pip
    sudo apt -y --no-install-recommends install sqlite3
    sudo apt -y --no-install-recommends install python3.10-venv
    sudo apt-get -y --no-install-recommends install libpq-dev
    sudo apt-get autoremove -yqq --purge
    sudo apt-get clean
    ```
### Install Airflow and initialize the DB
1. ssh into the instance
2. Create an airflow user:
    ```bash
    sudo adduser airflow
    sudo usermod -aG sudo airflow
    ```
3. As the airflow user, install airflow and init the DB
    ```bash
    su - airflow
    sudo pip install "apache-airflow[postgres,amazon,dbt.cloud,snowflake,slack,virtualenv,pandas,cncf.kubernetes]==2.6.1" --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-2.6.1/constraints-3.10.txt"
    airflow db init # note that this will create an airflow directory
    ```
### [OPTIONAL] Switch to an external DB and initialize it
1. Log in to the DB and create the Airflow database and user
    ```postgresql
    CREATE DATABASE airflow;
    CREATE USER airflow WITH PASSWORD 'airflow';
    GRANT ALL PRIVILEGES ON DATABASE airflow TO airflow
    ```
2. Edit the airflow/airflow.cfg file to replace the DB connection string and change the executor. If using the below commands, replace the username, password, arn, etc, with values for your RDS
    ```bash
    cd airflow
    sed -i 's#sqlite:////home/airflow/airflow/airflow.db#postgresql+psycopg2://user:password@database_arn/database_name#g' airflow.cfg
    sed -i 's#SequentialExecutor#LocalExecutor#g' airflow.cfg
    airflow db init
    ```
### Create the airflow admin login
```bash
airflow users create -u airflow -f airflow -l airflow -r Admin -e airflow@gmail.com
```

### Test the webserver and scheduler
```bash
airflow webserver &
airflow scheduler
```
1. After a minute or two, find the public IP of your instance (via the AWS console) and open \<public ip\>:8080 (e.g., 123.45.0.1:8080) in a web browser
2. Log in with the airflow admin credentials you created. You should see a list of example DAGs. You can try turning one on to test the scheduler
3. [OPTIONAL] To get rid of the sample DAGs on next launch, change the load_examples parameter in `airflow.cfg` to `False`
```bash
# from the airflow directory
sed -i 's#load_examples = True#load_examples = False#g' airflow.cfg
```
### [OPTIONAL] Fire up Airflow every time the instance is launched
1. In vi or another text editor open (with sudo) `/etc/systemd/system/airflow-webserver.service`
    ```bash
    sudo vi /etc/systemd/system/airflow-webserver.service
    ```
2. Paste in the following
    ```
    [Unit]
    Description=Airflow webserver daemon
    After=network.target postgresql.service
    Wants=postgresql.service
    [Service]
    EnvironmentFile=/etc/environment
    User=airflow
    Group=airflow
    Type=simple
    ExecStart= /usr/local/bin/airflow webserver
    Restart=on-failure
    RestartSec=5s
    PrivateTmp=true
    [Install]
    WantedBy=multi-user.target
    ```
3. Create another file called `/etc/systemd/system/airflow-scheduler.service`
4. Paste in the following
    ```
    [Unit]
    Description=Airflow scheduler daemon
    After=network.target postgresql.service
    Wants=postgresql.service
    [Service]
    EnvironmentFile=/etc/environment
    User=airflow
    Group=airflow
    Type=simple
    ExecStart=/usr/local/bin/airflow scheduler
    Restart=always
    RestartSec=5s
    [Install]
    WantedBy=multi-user.target
    ```
5. Enable the services and relaunch the instance
    ```bash
    sudo systemctl enable airflow-webserver.service
    sudo systemctl enable airflow-scheduler.service
    sudo systemctl start airflow-scheduler
    sudo systemctl start airflow-webserver
    ```
