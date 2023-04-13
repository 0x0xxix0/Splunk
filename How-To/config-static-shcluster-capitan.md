## SH01 (always must be a capitan)

``
splunk edit shcluster-config -mode capitan -capitan_uri <cluster_capitan_member_uri>:<management_port> -election false
``


At all other shcluster members:

## SH02 

``
splunk edit shcluster-config -mode member -capitan_uri <cluster_capitan_mamber_uri>:<manage_port> -election false
``

``
splunk restart
``


Make config for dynamic elections:

In all SH in cluster . <sh_own_uri> CHANGES FOR EACH MEMBERs
``
splunk edit shcluster-config -election true -mgmt_uri <sh_own_uri>:<manamage_port>
``

``
splunk restart
``


at member that must stay captan
splunk init shcluster-config -auth splunk:Changeme -mgmt_uri https://192.168.20.20:8089 -replication_port 9887 -replication_factor 3 -conf_deploy_fetch_url https://192.168.10.10:8089 -secret splunk -shcluster_label New_cluster   splunk restart
