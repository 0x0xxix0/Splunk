<h2> Налаштування  SSL Forwarding & Reciaving. </h2>


_$SPLUNK_HOME/etc/auth_ - Тут знаходяться всі стандартні сертифікати та ключі, які використовує  Splunk. 


Примітка: Дана конфігурація СА відбувається на Деплоймент Сервері, який обслуговуватиме приєднані до нього інстанси для forwarding та recieving.


## CA KEY AND CERTIFICATE

Для створення власних сертифікатів необхідно створити директорію $SPLUNK_HOME/etc/auth/<CAFolder>/

1) Для створення самопідписаних сертифікатів, перш за все необхідно створити __myCAPrivatKey.key__. Приватний ключ центру сертифікації (Найчастіше це окремий інстанс або Деплоймент сервер) <br>


 ``` 
 $SPLUNK_HOME/bin/splunk cmd openssl genrsa -aes256 -out $SPLUNK_HOME/etc/auth/<CAFolder>/myCAPrivateKey.key 2048 
 ```
 
На даному кроці необхідно ввести пароль для CA Privat Key. 
![image](https://user-images.githubusercontent.com/119075926/213775211-5498481f-9032-4ea0-936f-4a8f427afad4.png)


2) Наступним кроком є створення __myCACertificate.csr__ (Закритий сертифікат СА) використавши створений на попередньому кроці приватний ключ центру сертифікації.


```
$SPLUNK_HOME/bin/splunk cmd openssl req -new -key $SPLUNK_HOME/etc/auth/<CAFolder>/myCAPrivateKey.key -out $SPLUNK_HOME/etc/auth/<CAFolder>/myCACertificate.csr
```
Для початку створення  __myCACertificate.csr__ необхідно ввести пароль від __myCAPrivatKey.key__ який був створений на попередньому кроці. 


При перед останній та останній крок можна пропустити (проте це ішю для безпеки)

![image](https://user-images.githubusercontent.com/119075926/213777578-3d68e4f3-ff1d-4181-a751-8ebdfd7d39d2.png)

3) На даному кроці ми стовримо відкриті сертифікати СА __myCACertificate.pem__


```
$SPLUNK_HOME/bin/splunk cmd openssl x509 -req -in $SPLUNK_HOME/etc/auth/<CAFolder>/myCACertificate.csr -sha512 -signkey $SPLUNK_HOME/etc/auth/<CAFolder>/myCAPrivateKey.key -CAcreateserial -out $SPLUNK_HOME/etc/auth/<CAFolder>/myCACertificate.pem -days 1095
```

Цей сертифікат ми будемо розсилати на всі інстанси для того щоб вони могли довіряти даному центру сертифікації. 
При формуванні необхідно ввести пароль від __myCAPrivatKey.key__

![image](https://user-images.githubusercontent.com/119075926/213782444-c626def0-75bb-460a-8712-927062cf33f6.png)

Після виконання в нас повнні бути наступні файли:
 * $SPLUNK_HOME/etc/auth/<CAFolder>/CAPrivateKey.key - закритий ключ СА
 * $SPLUNK_HOME/etc/auth/<CAFolder>/CACertificate.csr - закритий сертифікат СА
 * $SPLUNK_HOME/etc/auth/<CAFolder>/CACertificate.pem - відкритий сертифікат СА
 
![image](https://user-images.githubusercontent.com/119075926/213789385-0be7bc30-14db-4bc3-a818-420ac314c29d.png)



## RECIEVER SERVER KEY AND CERTIFICATE


1) Тепер коли ми маємо все що описує наш СА (який буде підписувати наші сертифікати) можемо створювати __ServePrivateKey.key__
При генерації необхідно ввести пароль для __ServePrivateKey.key__


```
$SPLUNK_HOME/bin/splunk cmd openssl genrsa -aes256 -out ServePrivateKey.key 2048
```


![image](https://user-images.githubusercontent.com/119075926/213790857-88df8855-ed6f-417f-835b-7f730b4d37d9.png)


2) Наступним кроком ми створюємо закритий сертивікат сервера __PrivateServerCertificate.csr__
Створюємо його з допомогою стовреного на попередньому кроці приватного ключа сервера __ServerPrivateKey.key__


```
$SPLUNK_HOME/bin/splunk cmd openssl req -new -key ServerPrivateKey.key -out PrivateServerCertificate.csr
```


![image](https://user-images.githubusercontent.com/119075926/213791886-458c0ea2-ff02-4fd8-9efd-44fac143cd91.png)



3) З використанням __PrivateServerCertificate.csr__ ,  __CAPrivateKey.key__ та __CACertificate.pem__ генеруємо відкритий серверний сертифікат __OpenServerCertificate.pem__ 

При генерації необхідно ввести пароль від __CAPrivateKey.key__ 

```
$SPLUNK_HOME/bin/splunk cmd openssl x509 -req -in PrivateServerCertificate.csr -SHA256 -CA $SPLUNK_HOME/etc/auth/<CAFolder>/myCACertificate.pem -CAkey $SPLUNK_HOME/etc/auth/<CAFolder>/myCAPrivateKey.key -CAcreateserial -out OpenServerCertificate.pem -days 1095

```
![image](https://user-images.githubusercontent.com/119075926/213794039-e25b6311-6810-49ca-a931-6eff2010bb1f.png)

Після цих кроків в нас повинні бути наступні файли:
 * ServerPrivateKey.key -  приватний ключ сервера
 * PrivateServerCertificate.csr - приватний сертифікат сервера
 * OpenServerCertificate.pem - відкритий (підписаний) сертифікат серверу з допомогою СА

![image](https://user-images.githubusercontent.com/119075926/213796077-ccf6be7c-0d1e-40ec-af90-cb66c7d7fc81.png)

** An SRL file contains a serial number generated while signing an OpenSSL certificate. It is used to uniquely identify a signed certificate. SRL files use the same base filename as a certificate's

4) Тепер коли ми маємо все необхідне, потрібно створити один загальний сертифікат, щоб  Splunk міг його використовувати.
Він повинен містити __OpenServerCertificate.pem__ __ServerPrivateKey.key__ __myCACertificate.pem__ саме в такому порядку. 


```
cat myServerCertificate.pem myServerPrivateKey.key $SPLUNK_HOME/etc/auth/<CAFolder>/myCACertificate.pem > myNewServerCertificate.pem
```

![image](https://user-images.githubusercontent.com/119075926/213798100-163a934f-b7d2-4b5b-9180-3d45321b86fe.png)


5) Для налаштування ssl recieving створюємо app на Деплоймент сервері, для відправлення даних серифікатів на інстанси, які повинні виконувати recieving.
Відповідно то топології. Дана app називатиметься _<ssl-recieving>_. 

### Конструкція app _<ssl-recieving>_

```
ssl-recieving
|
|_auth
|  |_ ServerPrivateKey.key
|  |_ OpenServerCertificat.pem
|  |_ PrivateServerCertificate.csr
|  |_ myNewServerCertificate.pem 
|
|_local
  |_inputs.conf
 
```

### inputs.conf
```
[splunktcp-ssl:<splunk_port>]
disabled = false

[SSL]
serverCert = $SPLUNK_HOME/etc/apps/<ssl-recieving>/auth/myNewServerCertificate.pem
sslPassword = <password from ServerPrivateKey>
 
```


### Налаштування SSL Forwarding

1. Згенерувати новий приватний ключ для forwarder.


```
splunk cmd openssl genrsa -aes256 -out fwPrivate.key 4096
```

2. Виконати запит на створення закритого серитфікату.


```
splunk cmd openssl req -new -key fwPrivate.key -out fwPrivateCert.csr
```

3. Виконати запит на стоврення відкритого сертифікату.


```
splunk cmd openssl x509 -req -in fwPrivateCert.csr -SHA256 -CA /opt/splunk/etc/auth/<CAFolde>/myCACertificate.pem -CAkey /opt/splunk/etc/auth/<CAFolder/myCAPrivate.key -CAcreateserial -out fwPublicCert.pem -days 3650
```
 
В результаті в нас повинно бути 3 файли:
   * fwPrivate.key - Приватний ключ форвардера.
   * fwPrivateCert.csr - Приватний сертифікат.
   * fwPublicCert.pem - Підписаний СА відкритий сертифікат.


4. Обєднати відкритий сертифікат, приватний ключ та сертифікат СА в один файл.


```
cat forwarder.pem >> forwarder.pem
cat forwarder.key >> forwarder.pem 
cat $SPLUNK_HOME/etc/auth/<CAFolder>/myCACertificate.pem >> forwarder_r.pem 

```
 
5. Обєднати відкритий сертифікат СА та спланковий ааpsCA.

```
cat $SPLUNK_HOME/etc/auth/<CAFolder>/myCACertificate.pem  >> cacert.pem
cat $SPLUNK_HOME/etc/auth/appsCA.pem  >> cacert.pem
```
 
В результаті в нас повинно бути 5 файлів:
 * fwPrivate.key - Приватний ключ форвардера.
 * fwPrivateCert.csr - Приватний сертифікат.
 * fwPublicCert.pem - Підписаний СА відкритий сертифікат.
 * forwarder.pem - обєднаний для використання спланку сертифікати.
 * cacert.pem - обєднані СА сертифікати.

 
6. Надати відповідні права для <ssl-forwarding>

```
chmod -R 640 ssl-forwarding/ -R
chown splunk ssl-forwarding/ -R
chgrp splunk ssl-forwarding/ -R
```
