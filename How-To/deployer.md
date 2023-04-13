
Перед тим як пушити апки з деплоєра його треба перевести в пушш режим, для цього

##  Deployer
``

/opt/splunk/etc/system/local/app.conf - Globaly
/opt/splunk/etc/shcluster/<app>/local/app.conf - Localy
``  
``
[shclustering]
deployer_push_mode = [marge_to_default|full|local_only|default_only] 
``
<img width="2401" alt="Screenshot 2023-04-13 at 14 24 16" src="https://user-images.githubusercontent.com/119075926/231744210-f7d054a4-661b-4a46-a6e2-187c4693916e.png">


При використанні стандартних налаштувань апка буде пушитись тільки 1, потім до другого і так далі.
Якщо в server.conf

``
[shclustering]
deployerPushTreads = auto 
``


То апки будуть одночасно пушитись на всі 

  Командою запустити старт реплікації
   
``./splunk apply shcluster-bundle -target "<uri_sh01>:8089, <uri_sh02>:8089"
