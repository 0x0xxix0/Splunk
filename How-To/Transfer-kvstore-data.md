# Задача: З старого SearchHead пренести всі investigation в новий, щоб в залишились всі дані, які були присутні на новому.

Старий __SH old__

<img width="710" alt="Screenshot 2023-05-08 at 10 55 23" src="https://user-images.githubusercontent.com/119075926/236768198-06313c0d-bb66-403b-8caa-9635daaa3094.png">




Новий __SH new__ містить один інцидент, який створено _splunk_ 

<img width="2544" alt="Screenshot 2023-05-08 at 10 23 58" src="https://user-images.githubusercontent.com/119075926/236761380-baece0bf-aed0-45cf-b9e4-e8af2b24e479.png">




## Крок 1. Виконуємо бекап kvstore на обох SH. 

Doc in splunk wiki: https://docs.splunk.com/Documentation/Splunk/9.0.4/Admin/BackupKVstore

`./splunk backup kvstore -archiveName <archive>`


__SH old__

<img width="745" alt="Screenshot 2023-05-08 at 10 22 24" src="https://user-images.githubusercontent.com/119075926/236761045-11a0f1bf-16d1-47ea-b64e-29b007e81e3e.png">

__SH new__


<img width="902" alt="Screenshot 2023-05-08 at 11 02 44" src="https://user-images.githubusercontent.com/119075926/236769814-9722dca1-27e0-4768-9cef-648e628d6e10.png">

Тут назви архівів я робив однаковими, проте це не важливо.


## Крок 2. Перекидаємо бекап архів з старого на новий.

В результаті виконання команди на кроці 1 створиться файл `$SPLUNK_HOME/vat/lib/splunk/kvstorebackup/<archive>.tar.gz`
Його необхідно перекинути на хост _SH new_.


<img width="400" alt="Screenshot 2023-05-08 at 11 00 48" src="https://user-images.githubusercontent.com/119075926/236769376-67d2be65-c95b-48b9-a341-4b950dbe5cfc.png">




## Крок 3. Розархівовуємо обидва бекап архіви.

`tar -xvzf <archive_name>.tar.gz`


__SH old__

Тут заскрітнить забув, але все те саме тільки в іншій дирикторії.

__SH new__

<img width="898" alt="Screenshot 2023-05-08 at 11 04 49" src="https://user-images.githubusercontent.com/119075926/236770235-0799e099-f8a2-4087-a46e-6f1f10ea0aac.png">


##  Крок 4. Мерджимо інциденти.

Оскільки в нас задача перенести з _SH old_ тільки інциденти, необхідно виконати обєднання __SplunkEnterpriceSuite__  дирикторії, яка в собі містить всі дані про investigations.

<img width="964" alt="Screenshot 2023-05-08 at 11 08 48" src="https://user-images.githubusercontent.com/119075926/236771061-aab4d839-d10c-4d89-ac6c-6ea1062fae46.png">


Отже тут уважно: 

На __SH old__ вміст наступний.

<img width="964" alt="Screenshot 2023-05-08 at 11 08 48" src="https://user-images.githubusercontent.com/119075926/236771061-aab4d839-d10c-4d89-ac6c-6ea1062fae46.png">

В той час як на __SH new__ містяться такі файли.

<img width="751" alt="Screenshot 2023-05-08 at 11 13 07" src="https://user-images.githubusercontent.com/119075926/236771953-dde6087d-1982-4121-abf8-68ba9af8853a.png">

В нас мається однакова дирикторія __investigations__. Вона містить в собі наступні файли (які описують інцидент)

__SH old__

<img width="546" alt="Screenshot 2023-05-08 at 11 14 48" src="https://user-images.githubusercontent.com/119075926/236772308-289dfa22-b0ba-48c0-85aa-65f0a4490772.png">

__SH new__

<img width="807" alt="Screenshot 2023-05-08 at 11 15 32" src="https://user-images.githubusercontent.com/119075926/236772462-5154b40b-acd1-429a-8fb8-260ffbb4a288.png">

Імена однакові, проте ці файли описують різні інциденти.

__SH old__

<img width="1008" alt="Screenshot 2023-05-08 at 11 17 13" src="https://user-images.githubusercontent.com/119075926/236772794-bd6b17aa-1e66-4377-a8a5-3ed5ed0e7a62.png">

__SH new__

<img width="898" alt="Screenshot 2023-05-08 at 11 17 26" src="https://user-images.githubusercontent.com/119075926/236772830-2be06c35-77cb-4607-bec5-91f3681acd1c.png">

Змінюємо імя файлу на __SH new__ (тут пофіг головне щоб вони за неймінг конвеншн спланка були названі)
Перейменовуємо `.json` файл який на __SH new__.

<img width="892" alt="Screenshot 2023-05-08 at 11 23 13" src="https://user-images.githubusercontent.com/119075926/236774033-ec245009-9be1-442a-8faf-466f898eb56e.png">

Далі переносимо вміст SplunkEnterpriseSuite __SH old__  в дирикторію в __SH new__.

<img width="816" alt="Screenshot 2023-05-08 at 11 26 57" src="https://user-images.githubusercontent.com/119075926/236775021-3f7e860b-caf2-4201-aff1-36edfa8af147.png">


<img width="1013" alt="Screenshot 2023-05-08 at 11 26 44" src="https://user-images.githubusercontent.com/119075926/236774942-4204da6f-66f9-4866-bec6-21032da8fa3c.png">


<img width="802" alt="Screenshot 2023-05-08 at 11 27 23" src="https://user-images.githubusercontent.com/119075926/236775080-c4e563c0-7a4e-4a4f-9732-b4a510f2d358.png">

Також копіюємо всі додаткові дирикторії які присутні в SplunkEnterpriceSuite

<img width="984" alt="Screenshot 2023-05-08 at 11 31 36" src="https://user-images.githubusercontent.com/119075926/236775981-eec22ac8-6542-45db-b8ad-ecec83430053.png">


<img width="1017" alt="Screenshot 2023-05-08 at 11 32 43" src="https://user-images.githubusercontent.com/119075926/236776188-7f7d6a72-0c7b-489c-a283-02cdc0d32892.png">

<img width="694" alt="Screenshot 2023-05-08 at 11 33 01" src="https://user-images.githubusercontent.com/119075926/236776245-83dbeac6-50a5-487b-95bf-29849c9ac193.png">


##  Крок 5. Формуємо новий бекап архів.

`tar -cvzf test_backup.tar.gz *`

<img width="826" alt="Screenshot 2023-05-08 at 14 43 54" src="https://user-images.githubusercontent.com/119075926/236815308-7b3da13c-128b-4da2-ba3a-1dd32b13703d.png">



## Крок 6. Виконуємо відновлення з бекапу.

`splunk enable kvstore-maintenance-mode`


`./splunk restore kvstore -archiveName test_backup.tar.gz`

Перевіримо статус бекапування.

`./splunk show kvstore-status`

В процесі:

<img width="1040" alt="Screenshot 2023-05-08 at 14 49 49" src="https://user-images.githubusercontent.com/119075926/236816386-24f29bbd-ab67-4475-805e-62bef318f8fb.png">

Завершився:

<img width="691" alt="Screenshot 2023-05-08 at 11 43 42" src="https://user-images.githubusercontent.com/119075926/236778622-479f0c67-fd68-4efe-918b-be1675b3893d.png">


`splunk disable kvstore-maintenance-mode`


<img width="719" alt="Screenshot 2023-05-08 at 11 50 41" src="https://user-images.githubusercontent.com/119075926/236780462-a091f7c4-488e-496a-b58b-830790a6a205.png">

В результаті в нас збереглись всі інциденти з старого +  1 з нового.

