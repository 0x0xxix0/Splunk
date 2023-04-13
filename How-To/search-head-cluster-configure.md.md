
## 192.168.87.139
## Deployer:

``
nano /opt/splunk/etc/system/local/server.conf
``

``
[shclustering]
pass4SymmKey = <secret>
shcluster_lable = <cluster_name>
``  

```
[shclustering]
pass4SymmKey = splunk
shcluster_lable = New_cluster
```

<img width="921" alt="Screenshot 2023-04-13 at 08 53 26" src="https://user-images.githubusercontent.com/119075926/231666148-11d7eb63-ed57-4aad-b589-dc63b247f5c7.png">


``  
splunk restart
``
  
-- In all SH peer 
``
splunk init shcluster-config -auth <username>:<password> -mgmt_uri <URI>:<management_port> -replication_port <replication_port> -replication_factor <n> -conf_deploy_fetch_url <URL>:<management_port> -secret <secret>
``



## 192.168.87.130
## SH01:

`` 
splunk init shcluster-config -auth splunk:Changeme -mgmt_uri https://192.168.20.10:8089 -replication_port 9887 -replication_factor 3 -conf_deploy_fetch_url https://192.168.10.10:8089 -secret splunk -shcluster_label New_cluster  
``

<img width="1562" alt="Screenshot 2023-04-13 at 09 15 37" src="https://user-images.githubusercontent.com/119075926/231669994-753cb504-08a4-44ee-a0e9-0f3afb597663.png">

``
splunk restart
``

## 192.168.87.138
## SH02:

``
splunk init shcluster-config -auth splunk:Changeme -mgmt_uri https://192.168.20.20:8089 -replication_port 9887 -replication_factor 3 -conf_deploy_fetch_url https://192.168.10.10:8089 -secret splunk -shcluster_label New_cluster  
``
``
splunk restart
``

## 192.168.20.30
## SH03:
  
``  
splunk init shcluster-config -auth splunk:Changeme -mgmt_uri https://192.168.20.30:8089 -replication_port 9887 -replication_factor 3 -conf_deploy_fetch_url https://192.168.10.10:8089 -secret splunk -shcluster_label New_cluster  
``  
``
splunk restart
``


Active Capitan: 

``
splunk bootstrap shcluster-capitan -servers_list "<URI>:<management_port>,<URI>:<management_port>,<URI>:<management_port>" -auth <username>:<password>
``

``
splunk bootstrap shcluster-capitan -servers_list "https://192.168.20.10:8089,https://192.168.20.20:8089,https://192.168.20.30:8089" -auth splunk:Changeme
``
<img width="1523" alt="Screenshot 2023-04-13 at 09 22 13" src="https://user-images.githubusercontent.com/119075926/231671398-333e2299-131f-4281-beb6-d3f9cf3c3503.png">


<img width="1176" alt="Screenshot 2023-04-13 at 09 25 21" src="https://user-images.githubusercontent.com/119075926/231672046-4152ad4f-5d3b-4c6c-ba25-70cd458fc559.png">




<img width="497" alt="Screenshot 2023-04-13 at 09 26 55" src="https://user-images.githubusercontent.com/119075926/231672382-1b5190e9-0ca9-422b-aaf0-c7216caebc82.png">



## Add new pear to exist luster:

##192.168.87.140
##SH04 
``
splunk init shcluster-config -auth splunk:Changeme -mgmt_uri https://192.168.87.140:8089 -replication_port 9887 -replication_factor 3 -conf_deploy_fetch_url https://192.168.87.139:8089 -secret splunk -shcluster_label New_cluster  
``

``
splunk restart
``
<img width="1580" alt="Screenshot 2023-04-13 at 10 14 15" src="https://user-images.githubusercontent.com/119075926/231682506-05a9d0ac-35c2-423e-a557-1d8f1375be94.png">

``
splunk add shcluster-member  -current_member_uri <URI>:<manage_port>
``


<img width="1565" alt="Screenshot 2023-04-13 at 10 17 23" src="https://user-images.githubusercontent.com/119075926/231683283-f1e2d6da-9b3e-4ba3-9af0-e76d21831fe1.png">


<img width="1280" alt="Screenshot 2023-04-13 at 10 18 26" src="https://user-images.githubusercontent.com/119075926/231683500-f3bb656d-50b5-4c2d-8ff1-2dfe48bf02c0.png">






### Remove pear from SH custer:


## !!! In member with we want to remove. 

``
splunk remove shcluster-member
``
 <img width="1199" alt="Screenshot 2023-04-13 at 09 55 13" src="https://user-images.githubusercontent.com/119075926/231678109-593382ec-377f-4c5e-b331-426c8af67539.png">

<img width="1309" alt="Screenshot 2023-04-13 at 09 55 36" src="https://user-images.githubusercontent.com/119075926/231678234-1a28331c-64a5-4fb1-9896-ebba9b19f2e3.png">








# Splunk clean

```
splunk stop
```

``
splunk clean all
`` 
  


