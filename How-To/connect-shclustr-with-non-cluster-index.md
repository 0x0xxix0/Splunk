
## Deployer:




## SH02:

## Indexer:


1. Manualy Wolunk Web / updating conf file / splunk CLI command
2. Automatically (make config at 1 indexer and other just replicate)




## SH01: 

Для дозволу реплікації конфігурації СХ кластеру

``
server.conf

[raft_statemachine]
disabled = false
replicate_search_peers = true
``

``
splunk restart
``

## SH02: 

Для дозволу реплікації конфігурації СХ кластеру

``
server.conf

[raft_statemachine]
disabled = false
replicate_search_peers = true
``

``
splunk restart
``


## SH01 Web

<img width="1287" alt="Screenshot 2023-04-13 at 10 54 30" src="https://user-images.githubusercontent.com/119075926/231692259-39f4489b-4564-4da6-bde7-ac2d36e5cbce.png">

<img width="606" alt="Screenshot 2023-04-13 at 10 54 53" src="https://user-images.githubusercontent.com/119075926/231692361-d2d182f3-f887-4a10-aa52-976239e7bc70.png">

<img width="664" alt="Screenshot 2023-04-13 at 10 55 01" src="https://user-images.githubusercontent.com/119075926/231692395-8159358d-1ef2-4ee5-a1ae-49461701acbb.png">

і на 1 з СХ треба  добавити інексер як distrebuted search pear. далі вони реплікують конфіги















