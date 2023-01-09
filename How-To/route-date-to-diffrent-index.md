In this case we have inputs that send monitor data to indexer to index _uncusifide_. We need seperate data from diffrent clients. 
In result client01 must send data to index=sensor001, client02 send data to index=sensor002 all other client from 003-009 must be not indexed. 


<h1> Forwarder </h1>

__inputs.conf__
```
  [monitor:///opt/data/*.log]
  disabled = 0
  index = uncusifide
  sourcetype = client_data
```


<h1> Indexer </h1>

__index.conf__
```
  [uncusifide]
  coldPath = $SPLUNK_DB/uncusifide/colddb
  enableDataIntegrityControl = 0
  enableTsidxReduction = 0
  homePath = $SPLUNK_DB/uncusifide/db
  maxTotalDataSizeMB = 512000
  thawedPath = $SPLUNK_DB/uncusifide/thaweddb

  [sensor001]
  coldPath = $SPLUNK_DB/sensor001/colddb
  enableDataIntegrityControl = 0
  enableTsidxReduction = 0
  homePath = $SPLUNK_DB/sensor001/db
  maxTotalDataSizeMB = 512000
  thawedPath = $SPLUNK_DB/sensor001/thaweddb

  [sensor002]
  coldPath = $SPLUNK_DB/sensor002/colddb
  enableDataIntegrityControl = 0
  enableTsidxReduction = 0
  homePath = $SPLUNK_DB/sensor002/db
  maxTotalDataSizeMB = 512000
  thawedPath = $SPLUNK_DB/sensor002/thaweddb
  ```

__props.conf__
```
[client_data]
DATETIME_CONFIG =
LINE_BREAKER = ([\r\n]+)
NO_BINARY_CHECK = true
SHOULD_LINEMERGE = false
category = Custom
pulldown_type = 1
#If 
TRANSFORMS-drop = route-to-sensor001,route-to-sensor002,drop_sensor
```

__transforms.conf__
```
#Stanza to dropp data from sensor003-sensor009
[drop_sensor]
REGEX = client=sensor00[3-9]
DEST_KEY = queue
FORMAT = nullQueue

#Stanaz to move data from sensor001 --> index=sensor001
[route-to-sensor001]
REGEX = client=sensor001
DEST_KEY = _MetaData:Index
FORMAT = sensor001

#Stanaz to move data from sensor002 --> index=sensor002
[route-to-sensor002]
REGEX = client=sensor002
DEST_KEY = _MetaData:Index
FORMAT = sensor002
```


<h1> Generata test events </h1>

![image](https://user-images.githubusercontent.com/119075926/211401646-76ba87ed-4ee0-471f-bea2-17731943db70.png)

<h1> Results of configurations. </h1>

![image](https://user-images.githubusercontent.com/119075926/211401157-377cd04f-f1a6-484d-a5de-375b79562e69.png)


NOTE:
If in props.conf we will have 2 or more atributes with same name (or two or more witout name) active will be only last one.
