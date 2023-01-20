<h1> Налаштування  SSL Forwarding & Reciaving. </h1>




_$SPLUNK_HOME/etc/auth_ - Тут знаходяться всі стандартні сертифікати та ключі, які використовує  Splunk. 



## CA KEY AND CERTIFICATE
1) Для створення самопідписаних сертифікатів, перш за все необхідно створити __myCAPrivatKey.key__. Приватний ключ центру сертифікації (Найчастіше це окремий інстанс або Деплоймент сервер) <br>

 ``` 
 $SPLUNK_HOME/bin/splunk cmd openssl genrsa -aes256 -out myCAPrivateKey.key 2048 
 ```
На даному кроці необхідно ввести пароль для CA Privat Key. 
![image](https://user-images.githubusercontent.com/119075926/213775211-5498481f-9032-4ea0-936f-4a8f427afad4.png)

2) Наступним кроком є створення __myCACertificate.csr__ (Закритий сертифікат СА) використавши створений на попередньому кроці приватний ключ центру сертифікації.

```
$SPLUNK_HOME/bin/splunk cmd openssl req -new -key myCAPrivateKey.key -out myCACertificate.csr
```
Для початку створення  __myCACertificate.csr__ необхідно ввести пароль від __myCAPrivatKey.key__ який був створений на попередньому кроці. 

При передостанньій та останній крок можна пропустити (проте це ішю для безпеки)

![image](https://user-images.githubusercontent.com/119075926/213777578-3d68e4f3-ff1d-4181-a751-8ebdfd7d39d2.png)

3) На даному кроці ми стовримо відкриті сертифікати СА __myCACertificate.pem__

```
$SPLUNK_HOME/bin/splunk cmd openssl x509 -req -in myCACertificate.csr -sha512 -signkey myCAPrivateKey.key -CAcreateserial -out myCACertificate.pem -days 1095
```

Цей сертифікат ми будемо розсилати на всі інстанси для того щоб вони могли ?????
При формуванні необхідно ввести пароль від __myCAPrivatKey.key__

![image](https://user-images.githubusercontent.com/119075926/213782444-c626def0-75bb-460a-8712-927062cf33f6.png)

Після виконання в нас повнні бути наступні файли:
 * CAPrivateKey.key - закритий ключ СА
 * CACertificate.csr - закритий сертифікат СА
 * CACertificate.pem - відкритий сертифікат СА (який ми будемо шарити на всі інстанси для чого треба розібратись)
![image](https://user-images.githubusercontent.com/119075926/213789385-0be7bc30-14db-4bc3-a818-420ac314c29d.png)


## SERVER KEY AND CERTIFICATE


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
$SPLUNK_HOME/bin/splunk cmd openssl x509 -req -in PrivateServerCertificate.csr -SHA256 -CA myCACertificate.pem -CAkey myCAPrivateKey.key -CAcreateserial -out OpenServerCertificate.pem -days 1095
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
cat myServerCertificate.pem myServerPrivateKey.key myCACertificate.pem > myNewServerCertificate.pem
```
![image](https://user-images.githubusercontent.com/119075926/213798100-163a934f-b7d2-4b5b-9180-3d45321b86fe.png)

 
Тепер підготовлені сертифікати можна використовувати як для WEB так і для FORWARDING & RECIVING

## FORWARDING & RECIVING

### Налаштування SSL Receiving 

Для налаштування прийому зашифорованих даних необхідно мати.  Open


### Налаштування SSL Forwarding

