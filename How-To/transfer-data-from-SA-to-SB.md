SA 8.0 - тут індековані дані

MC

<img width="748" alt="Screenshot 2023-02-17 at 16 09 09" src="https://user-images.githubusercontent.com/119075926/219677718-11c9c64c-d596-4b75-8cad-e3918b377d47.png">

I1

<img width="665" alt="Screenshot 2023-02-17 at 16 09 33" src="https://user-images.githubusercontent.com/119075926/219677796-212e2de3-b444-4ff4-aebe-0cd6c71cb08b.png">


I2

<img width="682" alt="Screenshot 2023-02-17 at 16 09 51" src="https://user-images.githubusercontent.com/119075926/219677864-1a227654-b3e1-43f7-bc2e-7aa942d538f4.png">


SB 9.0 - тут даних нема

Налаштовані кластери як на SA так і на SB.

1. Створюємо  новий індекс на SB 

<img width="1539" alt="Screenshot 2023-02-17 at 16 22 48" src="https://user-images.githubusercontent.com/119075926/219680734-8cb79ef1-20ae-4e37-a2a8-b603c540eadd.png">

<img width="716" alt="Screenshot 2023-02-17 at 17 58 02" src="https://user-images.githubusercontent.com/119075926/219702932-a0384f4e-c002-4f1f-bc1a-0d5e2953f142.png">


2. Перевірка 

<img width="2534" alt="Screenshot 2023-02-17 at 17 32 55" src="https://user-images.githubusercontent.com/119075926/219697096-ee188082-d878-4c86-9dcf-04d36032f915.png">

<img width="2540" alt="Screenshot 2023-02-17 at 17 32 41" src="https://user-images.githubusercontent.com/119075926/219697039-b4aebd99-e961-4fb4-aba1-bb81e1090685.png">


3. викачка всіх файлів які належать індексу на   SA

<img width="661" alt="Screenshot 2023-02-17 at 17 33 48" src="https://user-images.githubusercontent.com/119075926/219697315-c7df2f14-5a04-477e-9793-27aafb58d19d.png">


4. тепер самий піздец .... розкинути викачані файли в правильні місця + перейменувати це на те що повинно бути (хз мб через сорстайп буде проблема, бо його нема... але не це ж просто поле і воно вже розпаршене хуйню зморозив.)

Окей погнали..
В нас є файли 
<img width="555" alt="Screenshot 2023-02-17 at 17 36 06" src="https://user-images.githubusercontent.com/119075926/219697841-f4fa9210-9faa-4b7e-8571-5766c99a24f1.png">
<img width="1155" alt="Screenshot 2023-02-17 at 17 46 08" src="https://user-images.githubusercontent.com/119075926/219700219-10afb71b-06c6-4aea-9901-54a83f2c24f5.png">
<img width="1171" alt="Screenshot 2023-02-17 at 17 46 27" src="https://user-images.githubusercontent.com/119075926/219700295-3522f473-4c0a-4280-9341-bc47ce13fc68.png">

Перед заливом я краще потушу 2 індексери щоб вони не почали реплікувати дані.

db ми повинні закинути в value:hot1

colddb ми повинні закинути в value:cold1 - в мене вони порожні тому поки залишаю без змін 

thaweddb можем не чіпати бо він пустий. 

datamodel_summary впринципіі.... хуй зна але закину в $SPLUNK_DB там де створилась ця папка
<img width="1909" alt="Screenshot 2023-02-17 at 17 57 09" src="https://user-images.githubusercontent.com/119075926/219702708-79df441e-d8a7-4fef-85bd-c3b7b7ab9c54.png">
<img width="1880" alt="Screenshot 2023-02-17 at 17 57 26" src="https://user-images.githubusercontent.com/119075926/219702766-3c8a6c67-95e4-4ee7-a849-90d18d623ae4.png">


.dat files
<img width="1669" alt="Screenshot 2023-02-17 at 17 48 42" src="https://user-images.githubusercontent.com/119075926/219700815-707853a6-8ef3-4fb1-b2d8-eefe6b8e7a4e.png">

<img width="1700" alt="Screenshot 2023-02-17 at 17 49 39" src="https://user-images.githubusercontent.com/119075926/219700996-ecad3f2d-e801-458e-bf14-07fd2eb53651.png">

START SPLUNK алілуя

<img width="2545" alt="Screenshot 2023-02-17 at 17 59 33" src="https://user-images.githubusercontent.com/119075926/219703307-9469ba24-42fb-4451-b9d6-afb1201077ca.png">

<img width="2515" alt="Screenshot 2023-02-17 at 18 00 45" src="https://user-images.githubusercontent.com/119075926/219703620-6f96172c-ca32-429d-8ac0-a4a045bffb0a.png">







