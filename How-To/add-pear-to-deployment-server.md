# Forwarder configuration

One of the first step to configuration forwarder, its connect it to deployment server.


_deploymentclient.conf_

```
[target-broker:deploymentServer]
targetUri = <deploymentServerIp>:8089 <-- deployment server ip

[deployment-client]
disabled = false
clientName = <deploymentNewName>
```
