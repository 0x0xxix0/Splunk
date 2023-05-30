# Data routing in Splunk using transforms props



| sourcetype  | short_sourcetype  | data_type  |
|---|---|---|
| cisco:estreamer:netflow  |  net |  *_netflow |
|  cisco:estreamer:file, cisco:estreamer:data, cisco:estreamer:ids | estr  |  *_ngfw |
| cisco:stealthwatch:alert, maltrail:alert  | stw, mlt  | *_ndr  |
| cisco:amp:event, CrowdStrike:Event:Streams:JSON  | amp, crowd  | *_edr  |
|  fm:statistics | mail  | *_mail  |



## props.conf


```TRANSFORMS-route_<short_sourcetype>_<org_domain> = route_<short_sourcetype>_<org_domain>```

## transforms.conf

```
[route_<short_sourcetype>_<org_domain>]
REGEX = <special regex with uniq client ID >
DEST_KEY = _MetaData:Index
FORMAT = <org_domain>_<data_type>
```


