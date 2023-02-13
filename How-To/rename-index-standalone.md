## Start situation

![image](https://user-images.githubusercontent.com/119075926/218533540-f402bcc2-3074-431c-ba8e-807bad0c0a00.png)


Зміни, які необхідно виконати.
sensor001 --> client001
sensor002 --> client002

### 1) Stop splunk

``` $SPLUNK_HOME/bin/splunk stop```

![image](https://user-images.githubusercontent.com/119075926/218534824-6e26602c-a3c8-4019-8362-37929b39d09c.png)


### 2) Змінюємо indexes.conf в відповідному місці де він визначений

Можливі шляхи ``$SPLUNK_HOME/etc/system/<app_name>/local/indexes.conf`` ``$SPLUNK_HOME/etc/system/local/indexes.conf``

![image](https://user-images.githubusercontent.com/119075926/218535479-eaae8b1c-e59d-404c-a654-51d41f5dfcef.png)

### 3) Змінюємо ``$SPLUNK_DB``

![image](https://user-images.githubusercontent.com/119075926/218536439-27224dff-2c4c-41a9-9893-1440b83554a8.png)

Примітка: Тут я тестово перейменую `.dat` файл адже може він і вимахувався при зміні індекса в класері 
"These .dat files hold the next bucket ID to be created for that index, and they aren't influenced by homePath etc. settings in indexes.conf."

![image](https://user-images.githubusercontent.com/119075926/218542815-6f6a4a79-f43d-4621-9028-b49bfabeba6b.png)

### 3) Start splunk

![image](https://user-images.githubusercontent.com/119075926/218544108-a8c51fef-8271-400c-aea6-511cb5a5af9c.png)

![image](https://user-images.githubusercontent.com/119075926/218544175-0bd5c19d-02b9-4077-8122-a27d58060fc4.png)


Примітка: Не забудь поміняти конфіги на форвардерах `inputs.conf`



