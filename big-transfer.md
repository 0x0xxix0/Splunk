Так переїжджамо на нові індекси. 

МАСТЕР КЛАСТЕР
1) подивитись як на нашому проді називаються volume та виправити в скріпті їх назви (за необхідністю)

<img width="1988" alt="Screenshot 2023-03-16 at 13 33 32" src="https://user-images.githubusercontent.com/119075926/225604540-6f54b22b-981e-4585-9c81-f9eb9b7b0835.png">

2) на МС виконуємо скрипт який генерить всі необхідні апки.
  ВАЖЛИВО: Щоб скріпт лежав в /managed-app/
  
<img width="407" alt="Screenshot 2023-03-16 at 13 34 07" src="https://user-images.githubusercontent.com/119075926/225604682-c74099da-b506-478f-8660-1fc386f52e07.png">


<img width="1943" alt="Screenshot 2023-03-16 at 13 35 01" src="https://user-images.githubusercontent.com/119075926/225604886-96b2141b-013c-4816-855f-5189b7c27891.png">

3) на вебці виконати перевірку бандлів та пуш апок. Кластер необхідно буде перезавантажити.
4) перевірити створення індесків на індексерах
<img width="591" alt="Screenshot 2023-03-16 at 13 35 24" src="https://user-images.githubusercontent.com/119075926/225604978-7c05e9dd-ec0d-48d4-8fc5-5748ed508515.png">

<img width="1904" alt="Screenshot 2023-03-16 at 13 55 34" src="https://user-images.githubusercontent.com/119075926/225609341-428630e8-c206-424a-8735-0a11514874c9.png">





НА ФОРВАРДЕРАХ
4) В кожній з апок вставити в пропс та трансформс  (estreamer на 2 форвардерах які менеджаться окремо та на ДМЗ ХФ01  також окремо КОР)
<img width="1043" alt="Screenshot 2023-03-16 at 13 36 44" src="https://user-images.githubusercontent.com/119075926/225605300-9127ac5e-ec01-41f0-9e8a-40cec4fb5492.png">


<img width="194" alt="Screenshot 2023-03-16 at 13 36 56" src="https://user-images.githubusercontent.com/119075926/225605339-77a070cf-6c30-4db3-899e-460155c08887.png">


- estreamer

props

<img width="1152" alt="Screenshot 2023-03-16 at 13 57 27" src="https://user-images.githubusercontent.com/119075926/225609699-330d79e2-1ac8-4378-b069-029249984b37.png">

transforms

НЕ ЗАБУТИ ВИПРАВИТИ РЕГУЛЯРКУ НА ДСЗІ де 2 сенсори та регулярку host {}

<img width="1173" alt="Screenshot 2023-03-16 at 13 58 38" src="https://user-images.githubusercontent.com/119075926/225609940-0c6b09a6-f9bf-4cf2-a951-6b0cbd7dbea7.png">

<img width="1229" alt="Screenshot 2023-03-16 at 14 03 38" src="https://user-images.githubusercontent.com/119075926/225611258-1a28b01d-a1e9-4d0f-b375-a3b560907792.png">


 - maltrail

props

<img width="1163" alt="Screenshot 2023-03-16 at 13 59 59" src="https://user-images.githubusercontent.com/119075926/225610298-28f8f1b6-5ffe-4307-9e26-89592584af53.png">

transforms
НЕ ЗАБУТИ ВИПРАВИТИ РЕГУЛЯРКУ НА ДСЗІ де 2 сенсори та регулярку host {}


<img width="1197" alt="Screenshot 2023-03-16 at 14 00 44" src="https://user-images.githubusercontent.com/119075926/225610478-69193cb2-2d62-4322-9dda-a59a0804093b.png">


<img width="1050" alt="Screenshot 2023-03-16 at 14 04 10" src="https://user-images.githubusercontent.com/119075926/225611364-8f2394de-c188-441e-88a4-d84b0d47bd6b.png">


stealthwatch
props

<img width="1166" alt="Screenshot 2023-03-16 at 14 01 37" src="https://user-images.githubusercontent.com/119075926/225610715-02edc836-2006-4b6f-9dc6-026e12a7f148.png">

НЕ ЗАБУТИ ВИПРАВИТИ РЕГУЛЯРКУ НА ДСЗІ де 2 сенсори та регулярку host {}
 
transforms

<img width="1213" alt="Screenshot 2023-03-16 at 14 02 34" src="https://user-images.githubusercontent.com/119075926/225610929-dd49849e-aa89-487d-9a54-af2fb66db2cf.png">

<img width="1079" alt="Screenshot 2023-03-16 at 14 04 29" src="https://user-images.githubusercontent.com/119075926/225611419-3c524af5-6de7-42de-9659-2f5db7bdd601.png">

5) перезавантажити форвардер для зміни конфігурації
6) 
<img width="669" alt="Screenshot 2023-03-16 at 14 04 55" src="https://user-images.githubusercontent.com/119075926/225611501-12a13892-9a9c-4016-9dff-3942799ef866.png">

6) перевірити правильність наповнення

ЕСТРИМЕР ПЕРЕВІРКА.

1. відправка рек тайпу, який не в переліку дозволених



3. новий сеноср
4. нормальний ректайп в ngfw
5. 71 rec_type в netflow

<img width="2112" alt="Screenshot 2023-03-16 at 14 37 57" src="https://user-images.githubusercontent.com/119075926/225619295-b9c27d55-d3c5-4ebc-89dd-9337ed39d70c.png">


<img width="1916" alt="Screenshot 2023-03-16 at 14 37 32" src="https://user-images.githubusercontent.com/119075926/225619223-0c90cb3a-ea5d-4a80-a0f3-b1874a4d65bd.png">


ПЕРЕВІРКА МАЛТРАЙ та СТЕЛСВОТЧ

1. неіснуючий сенсор
2. норм пакет для одної організації ма потрапити в один і той же індекм
<img width="2253" alt="Screenshot 2023-03-16 at 14 39 58" src="https://user-images.githubusercontent.com/119075926/225619780-482c463f-5b2f-4e2d-9edb-43397e77e644.png">
<img width="2129" alt="Screenshot 2023-03-16 at 14 43 18" src="https://user-images.githubusercontent.com/119075926/225620568-4e78e6bc-9cf4-4c49-bc96-8cd0aecfd198.png">

<img width="1909" alt="Screenshot 2023-03-16 at 14 43 38" src="https://user-images.githubusercontent.com/119075926/225620662-d0c05d01-6fab-45b0-8bcb-e05f575324c4.png">










