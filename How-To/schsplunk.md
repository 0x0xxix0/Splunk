
## 192.168.10.10
## Deployer:

``
# cd /opt/splunk/etc/system/local/server.conf
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

``  
splunk restart
``
  
-- In all SH peer 
``
splunk init shcluster-config -auth <username>:<password> -mgmt_uri <URI>:<management_port> -replication_port <replication_port> -replication_factor <n> -conf_deploy_fetch_url <URL>:<management_port> -secret <secret>
``



## 192.168.20.10
## SH01:

`` 
splunk init shcluster-config -auth splunk:Changeme -mgmt_uri https://192.168.20.10:8089 -replication_port 9887 -replication_factor 3 -conf_deploy_fetch_url https://192.168.10.10:8089 -secret splunk -shcluster_label New_cluster  
``
``
splunk restart
``

## 192.168.20.20
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

