![Bucket Roadmap](https://github.com/0x0xxix0/Splunk/assets/119075926/aee17a2a-2f7b-4c2d-bcb8-65ca7f5eb1ae)


<img width="1397" alt="Screenshot 2023-06-05 at 16 37 27" src="https://github.com/0x0xxix0/Splunk/assets/119075926/1eed4922-4e50-4494-9a7c-8a99b66a5353">


+ генерація даних 1 івент кожну хвилину.

# Тест 1.

При таких налаштуваннях в мене повинен бути 1 HOT бакет

<img width="634" alt="Screenshot 2023-06-05 at 16 39 05" src="https://github.com/0x0xxix0/Splunk/assets/119075926/61ec8ea7-46dd-4d75-9339-576365cbe149">

 Все так і є.
 
 
 
 
# Тест 2.

Я зарез змінюю крон на генерацію 1 івента за 3 хв

<img width="1347" alt="Screenshot 2023-06-05 at 16 46 10" src="https://github.com/0x0xxix0/Splunk/assets/119075926/d658c689-abad-48f3-ac20-d21c2fa002ef">
<img width="627" alt="Screenshot 2023-06-05 at 16 46 26" src="https://github.com/0x0xxix0/Splunk/assets/119075926/827dbdfe-dcee-41c3-aff8-607db26fec81">

За 4 хв в мене появиться 1 Warm бакет. оскільки maxHotIdleSecs

<img width="1341" alt="Screenshot 2023-06-05 at 16 48 32" src="https://github.com/0x0xxix0/Splunk/assets/119075926/5dd47010-814e-47c6-b356-677d97184379">

<img width="632" alt="Screenshot 2023-06-05 at 16 48 47" src="https://github.com/0x0xxix0/Splunk/assets/119075926/16ea5007-d844-4d5e-a672-9c8b1c27daa9">

Все так і є.


# Тест 3. 

Оскільки в мене може бути тільки  9 хв зявитись 3 Warm бакети. 

<img width="729" alt="Screenshot 2023-06-05 at 16 54 22" src="https://github.com/0x0xxix0/Splunk/assets/119075926/21ca58c3-e172-496e-96c2-1c38148a5e8f">


Через 12 хв добавиться 1 колд бакет.

<img width="1358" alt="Screenshot 2023-06-05 at 16 57 29" src="https://github.com/0x0xxix0/Splunk/assets/119075926/26577c19-3ab0-4792-a47d-ec770a37bf8a">

<img width="879" alt="Screenshot 2023-06-05 at 16 57 12" src="https://github.com/0x0xxix0/Splunk/assets/119075926/406b4d9b-b686-403f-bbe9-94dcdd27ebf1">



# Тест 4.

Я відключив прихід даних перший івент  

<img width="640" alt="Screenshot 2023-06-05 at 16 59 33" src="https://github.com/0x0xxix0/Splunk/assets/119075926/52c2f22d-348e-468a-8c0c-4bd717f23ee9">

<img width="501" alt="Screenshot 2023-06-05 at 17 00 17" src="https://github.com/0x0xxix0/Splunk/assets/119075926/757d2030-3343-4fc6-a8fa-11d24a33d5eb">

# Тест 5.
оскільки в нас дані при досягнені віку 20 хв підуть в фрозен.
Об 2:05:01 повинен перейти в фроузен


<img width="478" alt="Screenshot 2023-06-05 at 17 01 51" src="https://github.com/0x0xxix0/Splunk/assets/119075926/a27b6805-06df-4a62-a889-7601858d942b">


<img width="469" alt="Screenshot 2023-06-05 at 17 06 25" src="https://github.com/0x0xxix0/Splunk/assets/119075926/f21931bb-c327-4d6b-b71d-14e310897196">


Все так і є.


