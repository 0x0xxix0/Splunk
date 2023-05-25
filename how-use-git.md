## First steps


10 years ago

changed markup syntax, added summary of the site goal, getting starte…
# Learn.GitHub.com
This is the code and data behind [learn.github.com](http://learn.github.com). Its
primary goal is to be a course for you to follow and learn Git and GitHub on your own.
There are additional resources available once someone is beyond following this course
such as:
* [teach.github.com](http://teach.github.com) for teaching others.
* [training.github.com](http://training.github.com) for some instructor lead classes.
* [Git-SCM](http://git-scm.com) for some additional learning materials.
# Development
Working or running this site locally is easy! Just follow these steps:
```sh
$ git clone https://github.com/github/learn.github.com
$ cd learn.github.com
$ script/bootstrap
$ jekyll --server
#=> Open a browser and navigate to localhost:4000 and away you go!
```
# Contributing



# config


```
git config --global user.name "User Name"
```

```
git config --global user.email "user@email"
```


# project

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


# clone

Якщо проект знаходиться в центральному репозиторії (__remote__) для створення його локальної копії.

```
git clone <repo url>
```





## Збереження змін в репозиторіях.

# add

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




# commit

Створює знімок стану файлів які були добавлені __add__

```
git commit 
-m [message] - добавити коментар до коміту.
-a - коміт для всього проекту
-am [message] - коміт для всього проекту з повідомленням
--amend - при передачі даного ключа буде змінено останній коміт, а не стоврений новий
```

# push


Відправити зміни в проекті на віддалений репозиторій

```
git push <rep url>
```

Якщо 

```
git push -u <remote_name> <local_branch_name>
```
















