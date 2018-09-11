---
layout: post
title: Why security SIEMs to be a good idea.
---
Within this blogpost, I will describe what a SIEM is, the usage as a security monitoring tool and how businesses
can benefit from using one within their organization. 


#### [What is a SIEM?](#what-is-a-siem)

A SIEM is short for Security Information and Event Management. It combines the Security information management and the 
Security Event management into one system. It can aggregate data from multiple data sources, for instance server logs. 

Within this example, I will be using the ELK stack as a SIEM. The ELK stack is a combination of three applications: Elasticsearch, Logstash and Kibana. It can be used to monitor and 
alert administrators to possible security incidents. Within this example I will be using a clean install of Ubuntu 18.04 LTS desktop environment.
                                                    
Note that Ubuntu server will also work.

#### [Why use a SIEM](#why-a-siem)
Using a SIEM has multiple advantages

     * All data can be visualized effectively.
     * Analysts and System- and Security engineers have an overview of the threats and weaknesses of their systems.
     * A Siem can monitor logs, process them and alert the operators of the SIEM when necessary.
 

#### [Setting up the ELK stack](#setting-up-elk)
##### [Install Java](#install-java)
First you need to set up java. 

    ### first update the package repository. 
    sudo apt update
    
    ### Java 10 is not supported, so a lower version will be used. 
    sudo apt install openjdk-8-jdk
    
    #### now check whether java is correctly installed.
    java -version

##### [Install Kibana](#install-kibana)
Login to you server where you want to setup kibana

    
    # Download the deb package for ubuntu
    sudo wget https://artifacts.elastic.co/downloads/kibana/kibana-6.4.0-amd64.deb
    sudo dpkg -i kibana-6.4.0-amd64.deb

    
Now open /etc/kibana/kibana.yml. In the Kibana configuration file, find the line that specifies server.host, and replace the IP address ("0.0.0.0" by default) with "localhost":
    
    
    server.host: "localhost"
    # IP of the Elasticsearch server
    elasticsearch.url: "http://localhost:9200"


Restart the kibana service.
	
    sudo systemctl restart kibana

Then enable the service to start at boottime
    
    sudo systemctl daemon-reload
    sudo systemctl enable kibana    
    


##### [Install Elasticsearch](#install-elasticsearch)

    # Download the deb package for ubuntu
    sudo wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.deb
    
    #cd to the Download location if necessary.
    sudo dpkg -i elasticsearch-6.4.0.deb
    
This can also be achieved by doing

    wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -  
    echo "deb http://packages.elastic.co/elasticsearch/6.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-6.x.list
    sudo apt-get update
    sudo apt-get -y install elasticsearch   
    
Open /etc/elasticsearch/elasticsearch.yml  and change the following.
	
    network.host: localhost
 
Restart the Elasticsearch service.
	
    sudo service elasticsearch restart
    
Then, run the following command to start Elasticsearch on boot up:

    sudo systemctl daemon-reload
    sudo systemctl enable elasticsearch
    

##### [Install Logstash](#install-logstash)

    # Download deb package for ubuntu
    sudo wget https://artifacts.elastic.co/downloads/logstash/logstash-6.4.0.deb
    sudo dpkg -i logstash-6.4.0.deb
    
Now restart the service and enable it at boottime. 

    sudo systemctl restart logstash
    sudo systemctl enable logstash 
    
Configuring Logstash will be part of another blog post. 

##### [Install Nginx](#install-nginx)
The ELK stack needs Nginx in order to function correctly. If you have it installed already, you can reuse that instance. 

    sudo apt-get -y install nginx

now you have to create a admin user that can access the Kibana web interface. for this example I will be using the default "admin"

    sudo -v #sudo will update the user's cached credentials, authenticating the user's password if necessary.
    echo "admin:`openssl passwd -apr1`" | sudo tee -a /etc/nginx/htpasswd.users   
    
Now edit the default server

    sudo nano /etc/nginx/sites-available/default

And replace it with this: 

        server {
            listen 80;
    
            server_name localhost;
    
            auth_basic "Restricted Access";
            auth_basic_user_file /etc/nginx/htpasswd.users;
    
            location / {
                proxy_pass http://localhost:5601;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;        
            }
        }
   
Save it and exit. Kibana will now serve on localhost:5601 and use the htpasswd.users as login credentials. Now restart nginx.

    sudo systemctl restart nginx

If everything is set up correctly you should be able to access http://localhost:5601 and see your ELK instance up and running. 

More configuration will be coming in a later post!
    
    
    