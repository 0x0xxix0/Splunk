## Rename index `test` --> `client_syslog` with all data.

1. Stop Index-cluster.

<img width="1799" alt="Screenshot 2023-02-13 at 16 37 18" src="https://user-images.githubusercontent.com/119075926/218487139-d3acd69e-9392-4559-82c9-c13565279245.png">

<img width="1911" alt="Screenshot 2023-02-13 at 16 39 55" src="https://user-images.githubusercontent.com/119075926/218487759-0298b6dc-d64d-492f-aaa2-343a24bf1249.png">


## Master Cluster side 

В `/opt/splunk/etc/master-apps/<app_name>/local/indexes.conf` перейменувати `[<index_name>]` та `homePath clodPath thawedPath`

До:

<img width="354" alt="Screenshot 2023-02-13 at 16 43 14" src="https://user-images.githubusercontent.com/119075926/218488592-560f72de-2a35-463c-9646-01f79501ab49.png">

Після:

<img width="424" alt="Screenshot 2023-02-13 at 16 42 19" src="https://user-images.githubusercontent.com/119075926/218488369-a0762dea-d346-4103-b03e-49434e100ec9.png">

хз не вийшло пробую ще перейменувати папку 
<img width="516" alt="Screenshot 2023-02-13 at 17 04 48" src="https://user-images.githubusercontent.com/119075926/218494249-1fc6dfc8-5fc7-4c6d-af83-745dab52a493.png">



## Indexers side


Перейменувати репозиторій в `$SPLUNK_HOME/var/lib/splunk/` на всіх індексерах кластеру.

#### spi01

<img width="1057" alt="Screenshot 2023-02-13 at 16 47 45" src="https://user-images.githubusercontent.com/119075926/218489788-89edb4b2-5406-4049-a40c-ec0740913487.png">

#### spi02

<img width="1064" alt="Screenshot 2023-02-13 at 16 48 56" src="https://user-images.githubusercontent.com/119075926/218490093-53ea6095-fca0-4a49-9411-5a5bbcec030e.png">


!! Тут походу спочатку Мастер класер перезагрузити треба щоб індекс не пушнувся. + памятати про те що з форвардера можуть летіти логи в старий індек, також не ок
в мене створився заново директорія з декількома строками ???

## Start splunk


<img width="1914" alt="Screenshot 2023-02-13 at 16 51 05" src="https://user-images.githubusercontent.com/119075926/218490587-1d9ec010-0cfa-4a7d-b1cd-c9e34994adf9.png">


хуйня не вийшла, чогось все те саме 

пульнув ролбек
<img width="1581" alt="Screenshot 2023-02-13 at 17 16 25" src="https://user-images.githubusercontent.com/119075926/218496974-5a64151f-5208-4250-8cbc-34468a4ce2ba.png">



<img width="1797" alt="Screenshot 2023-02-13 at 17 19 15" src="https://user-images.githubusercontent.com/119075926/218497699-adf57632-deb2-482c-ba87-e9647732926e.png">

### Тут з моменту перезапуску + ренейм на МС + ренейм на індексері в `/var/`

Знайшов ще на індексері тереба походу перейменувати ``/opt/splunk/etc/pear-apps/`` ``filename`` and ``staza name``

spi01
<img width="582" alt="Screenshot 2023-02-13 at 17 38 59" src="https://user-images.githubusercontent.com/119075926/218502776-aedeef70-4512-4192-99e9-82836c062074.png">

spi02

<img width="496" alt="Screenshot 2023-02-13 at 17 41 51" src="https://user-images.githubusercontent.com/119075926/218503550-0f32b973-d554-4448-9a5a-16feb189728d.png">

і в самій цій апці

Перегружаю МС  (індексери поки офнуті)

Включаю індексери

індекс не видно



<img width="817" alt="Screenshot 2023-02-13 at 17 46 51" src="https://user-images.githubusercontent.com/119075926/218504783-7b4524ba-7f49-4a3c-a401-473b98a44b3d.png">



ХУЙ.....

Буду завтра пробувати кластер дізейбл


