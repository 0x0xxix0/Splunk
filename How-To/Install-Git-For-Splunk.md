# Встановлення та налаштування додатку для Splunk, для контролю версій конфігураційних файлів. 

Посилання на додаток: https://splunkbase.splunk.com/app/4182


## Налаштовуємо input 

<img width="802" alt="Screenshot 2023-05-29 at 14 15 19" src="https://github.com/0x0xxix0/Splunk/assets/119075926/24fd761e-4dc8-4de5-924a-038fb27c943a">

```
Name - Унікальне імʼя для даного input.
Interval - Інтервал в секундах, з яким буде відбуватись перевірка на зміни.
Index - Індекс куди потраплятимуть логи про виконання контрою версій.
Working directory - Дирикторія, яка буде моніторитись на зміни. 
Repository directory - Дирикторія, де буде ініціалізований .git/.
Include Btool output - Чи включати автоматично створені файли для контролю версій.
Btool .conf files - Перелік імен файлів (які в спланку мають розширення .conf) які повинні контролюватись на зміни
```


 ### Помилка №1. 

<img width="1699" alt="Screenshot 2023-05-29 at 14 28 45" src="https://github.com/0x0xxix0/Splunk/assets/119075926/0000fe23-c3e5-477b-b697-56383b7b9902">


<img width="942" alt="Screenshot 2023-05-29 at 14 28 58" src="https://github.com/0x0xxix0/Splunk/assets/119075926/9c5e9493-9641-49d2-9aa1-d814503771a9">

В _Repository directory_ необхідно вказати локальний _/.git/_ шлях.

Даний _Repository directory_ повинен бути створений 

```
cd /repo_name/

git init


# в такому випадку 
Repository directory = /repo_name/.git/

```


### Помилка №2.

<img width="1693" alt="Screenshot 2023-05-29 at 14 33 00" src="https://github.com/0x0xxix0/Splunk/assets/119075926/08090861-31de-453b-aa7f-2df2e82fc6ee">

<img width="612" alt="Screenshot 2023-05-29 at 14 33 23" src="https://github.com/0x0xxix0/Splunk/assets/119075926/b544bac2-69af-4360-ab72-400e4c7e77c3">

Для виправлення даної помилки, допомагло не `--global` а `--local` 

```
 git config --global user.email "you@example.com"
 git config --global user.name "Your Name"
  ```
  
Після виконання даних команд в `/repo_name/.git/config`  повинні зявитись наступі строки.

<img width="642" alt="Screenshot 2023-05-29 at 14 41 47" src="https://github.com/0x0xxix0/Splunk/assets/119075926/248e6cd7-3555-4e0a-ae98-02a979b81f7d">


### Помилка № 3

<img width="1714" alt="Screenshot 2023-05-29 at 14 43 16" src="https://github.com/0x0xxix0/Splunk/assets/119075926/7d126d92-745e-4594-a693-ffeffd18fc5b">

<img width="671" alt="Screenshot 2023-05-29 at 14 43 44" src="https://github.com/0x0xxix0/Splunk/assets/119075926/c7ce8c00-96d4-4d05-a342-555ef7cbd990">


#### 1 

Виконано добавлення ssh-rsa ключ в git. 

```
ssh-keygen -t rsa -b 4096
```

<img width="610" alt="Screenshot 2023-05-29 at 14 47 42" src="https://github.com/0x0xxix0/Splunk/assets/119075926/8a7cd895-c289-448e-aee9-9feea2784f82">

```
cat /root/.ssh/id_rsa.pub
```

Завантажити в GitHub.

<img width="1114" alt="Screenshot 2023-05-29 at 14 48 24" src="https://github.com/0x0xxix0/Splunk/assets/119075926/8ffc83c8-bd21-435e-b1aa-10f0e237c706">


#### 2

```
git remote add origin git@github.com:0x0xxix0/SplunkArchecture.git
git push
git push --set-upstream origin master
```

<img width="697" alt="Screenshot 2023-05-29 at 14 53 37" src="https://github.com/0x0xxix0/Splunk/assets/119075926/3c9a99c5-5ac0-4aa9-b8cd-7b5a22219758">













