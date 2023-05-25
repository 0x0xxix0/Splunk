## First steps





## config

Локальний: /.git/config — налаштування на рівні репозиторію [--local].
Глобальний: /.gitconfig — налаштування. на рівні користувача. Тут зберігаються всі налаштування з  [--global].
Системний: $(prefix)/etc/gitconfig — налаштування на рівні всієї системи [--system].


```
git config --global user.name "User Name"
```

```
git config --global user.email "user@email"
```


Також можливо відкрити налаштування в текстовому редакторі 

```
git config [--global] --edit
```


## clone

Якщо проект знаходиться в центральному репозиторії (__remote__) для створення його локальної копії.

```
git clone <repo url>
```



## project (init)

Створення нового проекту.

```
git init projectName
```

Це стовриить дирикторію projectName яка буде містити наші файли.

<img width="1902" alt="Screenshot 2023-05-25 at 09 20 35" src="https://github.com/0x0xxix0/Splunk/assets/119075926/5a01cb65-6a10-4c6e-98c4-e4ec3a92b0ac">


Якщо потрібно створити проект в вже існуючій папці з кодом
```
git init
```







# Збереження змін в репозиторіях.

## add

При створеннні файлів в даній дирикторії необхідно добавити даний файл до __контролю версії__

```
git add file1.txt
```

<img width="604" alt="Screenshot 2023-05-25 at 09 22 26" src="https://github.com/0x0xxix0/Splunk/assets/119075926/36952736-1a0e-4a19-b1d8-95c6200da1c9">

Це сигналізує що нам необхідно добавити даний файл буде приймати участь в начступному __commit__  та контролювати його версії.


Наступна команда привяже віддалений репозиторій до локального.

```
git remote add <remote_name> <remote_repo_url>
```

Після привʼязки віддаленого репозиторію в нього можна відправляти (дивись __push__) локальні гілки (__branch_)




## commit

Створює знімок стану файлів які були добавлені __add__

```
git commit 
-m [message] - добавити коментар до коміту.
-a - коміт для всього проекту
-am [message] - коміт для всього проекту з повідомленням
--amend - при передачі даного ключа буде змінено останній коміт, а не стоврений новий
```







## push

Відправити зміни в проекті на віддалений репозиторій

```
git push <rep url>
```

Якщо виконана привʼязка до віддаленого репозиторію, то наступна команда перемістить __branch__ з іменем _<local_branch_name>_ в віддалений репозиторій _<remote_name>_

```
git push -u <remote_name> <local_branch_name>
```



## branch 


```
git branch - використовується для перегляду та переходу на інші гілки

```

# Відкат до попереднього стану

## checkout 

Виконує переміщення стану який був в __commit__

```
git checkout <commit_ID>
```

Даний спосіб завантажує один з __commit__ на компютер. В локальному компі ми повернемось до стану який був в цьому commit.
 
<img width="930" alt="Screenshot 2023-05-25 at 12 17 20" src="https://github.com/0x0xxix0/Splunk/assets/119075926/9de02379-868e-4d0c-a3ec-1c12972e372b">

При даному методі мітка __HEAD__ буде відкріплена від останнього (верхнього коміту)

Для відміни попереднього _commit_ цим методом
1. виконується перехід в місце до якого треба відкатитись
2. створюється нова гілка яка стає основною і не містить непотрібного commit

```
git checkout -b new_branch_without_crazy_commit
```
3. видаляється __branch__ з непотрібним __commit__
 
 
 ## revert
 
 В даному способі відміна відбувається створенням ще одного commit, який відповідатиме реверсу непотрібного.
 
 <img width="930" alt="Screenshot 2023-05-25 at 12 40 29" src="https://github.com/0x0xxix0/Splunk/assets/119075926/3063b2c4-3b9a-4e28-a8d8-cc82ec5abe4c">




































# Перевірка репозиторію


## status

Показує стан робочого каталогу та розділу проіндексованих файлів. 

<img width="718" alt="Screenshot 2023-05-25 at 11 24 23" src="https://github.com/0x0xxix0/Splunk/assets/119075926/aad48b5e-52d9-4bc1-adc5-4a2eb8e55cd5">

## log

Виводить інформацію про зміни __commit__

```
git log 
-n [limit] - кількість commit які повинні бути відображені.
--oneline -  вивести інформацію про коміт в одну лінію (так набагато зручніше)
--graph - створює основоану на тексті діаграму комітів.
--decorate - добавляє відображення branch або tag
--stat -  виводить додаткові дані, про те які файли та як змінювались в даному commit
-p - виводить патчі, які відповідають певним commit
--author [name] - commit які виконав саме цей автор
--grep [pattern] - пошук паттерна по повідомленню (саме тому важливо робити піздаті коменти в git commit -m "Comment"
<since>..<until> - відображає тільки коміти в діапазоні значень, дані аргументи можуть бути ID commit,branch names, HEAD etc.
<file> - перегляд історії конкретного файлу
<branche_name> - коміти тільки цієї branch
--branches [name] - 
```

<img width="656" alt="Screenshot 2023-05-25 at 11 24 03" src="https://github.com/0x0xxix0/Splunk/assets/119075926/eb18bbef-5a6e-4c0e-a3c2-1d0cc9181008">


<img width="666" alt="Screenshot 2023-05-25 at 11 29 09" src="https://github.com/0x0xxix0/Splunk/assets/119075926/d4b7230f-4c4e-47f0-ac7d-1822b411a132">


<img width="658" alt="Screenshot 2023-05-25 at 11 30 27" src="https://github.com/0x0xxix0/Splunk/assets/119075926/185eff6e-e10d-408c-898f-c99d416cbd7b">


<img width="663" alt="Screenshot 2023-05-25 at 11 32 39" src="https://github.com/0x0xxix0/Splunk/assets/119075926/a5bd0341-4e12-44c4-b112-6497e2f01b56">

<img width="665" alt="Screenshot 2023-05-25 at 11 36 38" src="https://github.com/0x0xxix0/Splunk/assets/119075926/6c8d38c6-2481-4f6d-9837-dab288920f38">

<img width="625" alt="Screenshot 2023-05-25 at 11 38 17" src="https://github.com/0x0xxix0/Splunk/assets/119075926/f4bcb317-15b6-473b-a0df-8e3fef5678e1">

<img width="806" alt="Screenshot 2023-05-25 at 11 47 48" src="https://github.com/0x0xxix0/Splunk/assets/119075926/0c5c4850-28ef-4f42-95da-8cb97c3f23c5">

<img width="654" alt="Screenshot 2023-05-25 at 11 48 57" src="https://github.com/0x0xxix0/Splunk/assets/119075926/f677df0f-556f-4d04-9baf-6f4dc6b825da">


<img width="707" alt="Screenshot 2023-05-25 at 11 50 34" src="https://github.com/0x0xxix0/Splunk/assets/119075926/fec0ec72-1440-4311-b4b9-a32eeb8de54e">



















