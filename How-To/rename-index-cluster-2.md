## Starting data

__Завдання:__ інснуючий індекс `test` який створений в кластері перейменувати в `client` з збереженням всіх даних, які були в попередньому індексі.

#### Search head.

<img width="1917" alt="Screenshot 2023-02-14 at 11 17 17" src="https://user-images.githubusercontent.com/119075926/218691891-d0787b49-6e5e-4b1e-97da-6f102f2c62ed.png">

#### Deployment Server

Інпутс який заповнює індекс `test` визначений в `<auditd_diia>`

<img width="1885" alt="Screenshot 2023-02-14 at 11 32 10" src="https://user-images.githubusercontent.com/119075926/218695534-c4f3d807-876c-4d1e-b1f1-b4aef174d770.png">


#### Master Cluster

`$SPLUNK_HOME/etc/master-apps`

<img width="404" alt="Screenshot 2023-02-14 at 11 18 37" src="https://user-images.githubusercontent.com/119075926/218692198-77d28310-00f0-47a1-afcf-28b72efef788.png">

`$SPLUNK_HOME/etc/master-apps/<app_name>/local/indexes.conf`

```
[test]
repFactor=auto
homePath=$SPLUNK_DB/test/db/
coldPath=$SPLUNK_DB/test/colddb/
thawedPath=$SPLUNK_DB/test/thaweddb/
```

<img width="526" alt="Screenshot 2023-02-14 at 11 19 56" src="https://user-images.githubusercontent.com/119075926/218692525-982d3f62-7ed3-4ea3-aac2-121f3d11a376.png">

Web index cluster

<img width="1905" alt="Screenshot 2023-02-14 at 11 20 47" src="https://user-images.githubusercontent.com/119075926/218692732-28679af7-ecd9-41dd-ba9e-72dea06b3f28.png">

#### Indexers

  ##### Spi01

  `$SPLUNK_HOME/etc/peer-apps/<apps_name>`
  
<img width="405" alt="Screenshot 2023-02-14 at 11 22 03" src="https://user-images.githubusercontent.com/119075926/218693017-59df13b9-8bfa-4aa9-9453-4fb759f7a301.png">

  `$SPLUNK_HOME/var/lib/splunk/<index_name>/`

<img width="497" alt="Screenshot 2023-02-14 at 11 23 12" src="https://user-images.githubusercontent.com/119075926/218693291-acfc45a9-6767-424e-8598-857f6f41a004.png">

  `$SPLUNK_HOME/var/lib/splunk/<index_name>.dat`

<img width="420" alt="Screenshot 2023-02-14 at 11 23 37" src="https://user-images.githubusercontent.com/119075926/218693399-a6a5994c-d3eb-4b1e-827d-7753e9c1900e.png">


  ##### Spi02

`$SPLUNK_HOME/etc/peer-apps/<apps_name>`

<img width="430" alt="Screenshot 2023-02-14 at 11 24 18" src="https://user-images.githubusercontent.com/119075926/218693560-c1a33b6b-56bd-40ae-9ad2-5f0cf055f623.png">

`$SPLUNK_HOME/var/lib/splunk/<index_name>/`

<img width="490" alt="Screenshot 2023-02-14 at 11 24 47" src="https://user-images.githubusercontent.com/119075926/218693688-15791e37-63a2-40fa-bc63-22b83fe40dcb.png">

`$SPLUNK_HOME/var/lib/splunk/<index_name>.dat`

<img width="436" alt="Screenshot 2023-02-14 at 11 25 09" src="https://user-images.githubusercontent.com/119075926/218693768-f780f0c1-87ba-4819-bc97-cb25cea8d55c.png">

### Heavy forwarder

<img width="524" alt="Screenshot 2023-02-14 at 11 29 12" src="https://user-images.githubusercontent.com/119075926/218694769-170683c9-848d-4d5b-9507-a6c9f7da8820.png">



# Process of renaming

## 1) Stop All Index Cluster

##### MC

<img width="536" alt="Screenshot 2023-02-14 at 11 29 53" src="https://user-images.githubusercontent.com/119075926/218694949-a6fa1a2b-021e-4f78-9669-022e216bbcf4.png">

##### Spi01

<img width="545" alt="Screenshot 2023-02-14 at 11 30 11" src="https://user-images.githubusercontent.com/119075926/218695017-2a9b18eb-cd7c-46eb-b3ac-1a4433f4d129.png">

##### Spi02

<img width="490" alt="Screenshot 2023-02-14 at 11 30 24" src="https://user-images.githubusercontent.com/119075926/218695070-95e75980-054c-4a41-b8dd-ac13b3b08b75.png">

## 2) Rename Forwarder inputs (MB disable this in hight priority becouse data will be at queue) or disable this inputs

  ### Deployment server

  Змінюємо назву індекса, в який тепер будуть надсилатись дані.
  
 <img width="234" alt="Screenshot 2023-02-14 at 11 34 36" src="https://user-images.githubusercontent.com/119075926/218696118-2e3d6309-e0f6-4c44-8f72-5851a1e73c14.png">

  Та виконуємо пуш оновленої апки, або очікуємо коли HeavyForwarder автоматично підтягне зміни (default 5min).
  
  ### HeavyForwarer
  
   Після змін.

<img width="524" alt="Screenshot 2023-02-14 at 11 41 55" src="https://user-images.githubusercontent.com/119075926/218697838-817f8377-3f76-47e3-8a0e-31ebf389390f.png">

  
## 3) Reame app and content of app in MC side

> Примітка: насправді імя папки тут не грає ролі, проте варто це робити щоб знати налаштування якого індексу де знаходиться, та не вести 1 індекс файл в 1 апці.
 
<img width="444" alt="Screenshot 2023-02-14 at 11 44 23" src="https://user-images.githubusercontent.com/119075926/218698450-14fadf3c-dcb3-473b-95c8-a70e8c5389ab.png">

Змінити `indexes.conf`

<img width="546" alt="Screenshot 2023-02-14 at 11 45 45" src="https://user-images.githubusercontent.com/119075926/218698831-fa5fd72d-6c3a-4343-ac6e-575fe38f95ed.png">



## 4) Indexes side: Rename `/etc/pear-app/` name app and `indexes.conf` content 

##### Spi01

<img width="549" alt="Screenshot 2023-02-14 at 12 01 49" src="https://user-images.githubusercontent.com/119075926/218702882-9762bbd5-3296-4bfc-b5f0-080f0468996c.png">

<img width="566" alt="Screenshot 2023-02-14 at 12 03 04" src="https://user-images.githubusercontent.com/119075926/218703205-63a557ae-e85c-4ecf-b710-9eab939bda13.png">

##### Spi02

<img width="446" alt="Screenshot 2023-02-14 at 12 03 52" src="https://user-images.githubusercontent.com/119075926/218703406-b607e705-9c5d-4dd2-857e-9577efb965e0.png">

<img width="562" alt="Screenshot 2023-02-14 at 12 04 44" src="https://user-images.githubusercontent.com/119075926/218703620-c9efbd6d-3ba2-410a-a7fb-ef58a3715fd9.png">


## 5) Rename .dat and $SPLUNK_DB content 

##### Spi01

> Примітка: якщо не виконувалась зміна назви апки на МС  (Примітка в пункті 3) цей крок варто пропустити, та залишити стару назву.

> Примітка: якщо індекс складає дані в інше місце, зміни його там ! (куди складає визначено в `inputs` пункт Starting data --> Master Cluster --> indexes.conf)

`$SPLUNK_DB/<app_name>`

<img width="1059" alt="Screenshot 2023-02-14 at 11 49 42" src="https://user-images.githubusercontent.com/119075926/218699838-cff04f27-5dee-48d7-8f2c-376f627e2930.png">

> Примітка: цей вайл та додаткова директорія, з даними Дата моделі всерівно будуть знаходитись тут навіть якщо ви зберігаєте дані індексека в іншому місці. Тому перейменовуємо файл та існуючу директорію.

`.dat file`

<img width="1057" alt="Screenshot 2023-02-14 at 11 53 19" src="https://user-images.githubusercontent.com/119075926/218700727-14785cc2-6fa2-4262-b67c-ee6012855601.png">



##### Spi02

`$SPLUNK_DB/<app_name>`

<img width="1068" alt="Screenshot 2023-02-14 at 12 00 30" src="https://user-images.githubusercontent.com/119075926/218702499-004b833b-cd79-4d18-96ad-d371170b14a8.png">

`.dat file`

<img width="1061" alt="Screenshot 2023-02-14 at 12 00 55" src="https://user-images.githubusercontent.com/119075926/218702625-257d0bc2-f8ea-4ffa-94b4-75d70eb32543.png">



## Start all splunk cluster instanse 


MC

<img width="544" alt="Screenshot 2023-02-14 at 12 11 04" src="https://user-images.githubusercontent.com/119075926/218705092-01a468e4-68bf-4319-9a81-1245614bd72c.png">

Spi01

<img width="554" alt="Screenshot 2023-02-14 at 12 10 30" src="https://user-images.githubusercontent.com/119075926/218704967-f8cefe84-6bc4-4733-9c44-8457687ce302.png">

Spi02

<img width="549" alt="Screenshot 2023-02-14 at 12 10 44" src="https://user-images.githubusercontent.com/119075926/218705019-c940bdb1-3b41-455c-950f-a9aeafdb34bc.png">


Results at UI

MC:

<img width="1908" alt="Screenshot 2023-02-14 at 12 18 05" src="https://user-images.githubusercontent.com/119075926/218706743-5c0038d6-76c1-4c73-99e2-d7541332bb8e.png">



Indexer:

  spi01
  
  <img width="1899" alt="Screenshot 2023-02-14 at 12 13 49" src="https://user-images.githubusercontent.com/119075926/218705767-99c9c3f2-7971-4116-965f-f025e8772939.png">

  
  spi02
  
  <img width="1890" alt="Screenshot 2023-02-14 at 12 14 27" src="https://user-images.githubusercontent.com/119075926/218705891-2d8f44c6-963c-4fb1-ba03-d67174645cf8.png">

SH:

<img width="1901" alt="Screenshot 2023-02-14 at 12 19 34" src="https://user-images.githubusercontent.com/119075926/218707093-3de60aca-eaa3-4665-abd5-ca9a33631608.png">


<img width="1909" alt="Screenshot 2023-02-14 at 12 19 57" src="https://user-images.githubusercontent.com/119075926/218707157-cba6beac-8737-4b4f-b789-f518bfacd4f1.png">




