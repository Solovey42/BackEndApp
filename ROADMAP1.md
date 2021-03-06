# План

### 1.Создать репозиторий на гитхабе - 5 мин

### 2.Описать тестовые данные - 20 мин

2.1 Описать пользователей - 10 мин

| №   | Пользователь| Пароль|
|:---:| :---------: |:-----:| 
| U1  | vasya       | 123   | 
| U2  | admin       | admin |
| U3  | q           | ?!@   | 
| U4  | abcd        | qwerty|

2.2 Описать тестовые ресурсы - 10 мин

| №     | Ресурс|Роль|Пользователь|
|:-----:|:-----:|:-----:|:---:|
|R1     |A	|READ	|vasya|
|R2	|A.B.C	|WRITE	|vasya|
|R3	|A.B	|EXECUTE|admin|
|R4	|A	|READ	|admin|
|R5	|A.B	|WRITE	|admin|
|R6	|A.B.C	|READ	|admin|
|R7	|B	|EXECUTE|q    |
|R8	|A.A.A	|EXECUTE|vasya|

### 3.Записать тесты в bat/sh файл R1.12 - 30 мин 

| №| Команда| Результат             |
|:--:| :-------------: |:------------------:| 
|T1.1	|`app.jar`	|1 + вывод справки|
|T1.2	|`app.jar -h`|	1 + вывод справки|
|T1.3	|`app.jar -q`|	0 + вывод справки (особый случай)|
|T2.1	|`app.jar -login vasya -pass 123`|	0|
|T2.2	|`app.jar -pass 123 -login vasya`|	0|
|T2.3	|`app.jar -login VASYA -pass 123`|	2|
|T2.4	|`app.jar -login asd -pass 123`|	3|
|T2.5	|`app.jar -login admin -pass 1234`|	4|
|T2.6	|`app.jar -login admin -pass admin`|	0|
|T3.1	|`app.jar -login vasya -pass 123 -role READ -res A`|	0|
|T3.2	|`app.jar -login vasya -pass 123 -role DELETE -res A`|	5|
|T3.3	|`app.jar -login vasya -pass 123 -role WRITE -res A`|	6|
|T3.4	|`app.jar -login vasya -pass 123 -role READ -res A.B`|	0|
|T3.5	|`app.jar -login admin -pass admin -role WRITE -res A.B.C`|	0|
|T3.6	|`app.jar -login vasya -pass 1234 -role DELETE -res A`	|4|
|T3.7	|`app.jar -login vasya -pass 123 -role WRITE -res A.B.C`	|6|
|T3.8	|`app.jar -login admin -pass admin -role READ`	|0 (удачная аутентиф.)|
| T3.9	|`app.jar -login admin -pass admin -role EXECUTE -res A`|	6|
| T3.10	|`app.jar -login admin -pass admin -role WRITE -res A.A`	|6|
|T4.1   |`app.jar -login vasya -pass 123 -role READ -res A -ds 2020-01-01 -de 2020-02-01 -vol 20`|	0
|T4.2	|`app.jar -login vasya -pass 123 -role READ -res A -ds 2020-01-01 -de 2020-15-01 -vol 20`|	7 (несуществующая дата)|
|T4.3	|`app.jar -login vasya -pass 123 -role READ -res A -ds 2020-01-01 -de asd -vol 20`|	7 (неправильный ввод даты)|
|T4.4   |`app.jar -login vasya -pass 123 -role READ -res A -ds 2020-01-01 -de 2020-02-01 -vol asd`|	7 (неправильный ввод объема)|
|T4.6	|`app.jar -login vasya -pass 123 -role READ -res A -ds 2020-01-01 -de 2020-02-01 -vol -1`|	7 (неверный объем)|

### 4.Простые сценарии - 30 мин

4.1 Создаем метод проверки наличия аргументов (R1.8.1) - 10 мин

4.2 Создаем метод вывода справки - 10 мин

4.3 Проверяем надо ли выводить справку `args[0] == “-h”` (R1.8.2) - 5 мин 

4.4 Во всех остальных случаях выводим справку и возвращаем 0  - 5 мин

### 5.Сценарии аутентификации - 88 минут

5.1 Создаем метод который проверяет надо ли аутентифицировать `args[0] == "login" && args[2] == "-pass"`  - 5 мин

5.2 Создаем метод validate login (проверяем правильность формата логина через regexp) (R1.1) - 5 мин

5.3 Создаем метод который проверяет существует ли логин (тест: login == vasya) - 5 мин

5.4 Создаем метод который проверяет пароль  (тест: login == vasya, pass == 123) - 5 мин

5.5 Создаем класс(data class) `User(login, pass)` - 5 мин

5.6 Создаем коллекцию для хранения юзеров и добавляем туда тестовые данные - 10 мин

5.7 Исправляем методы на работу с коллекцией юзеров - 20 мин

5.8 Создаем `data class ArgHandler (h, login, pass)` конструктор которого на вход получит массив параметров командной строки - 8 мин(R1.8)

5.9 В класс ArgHandler вносим методы проверки наличия аргументов (п 4.1), вывода справки (п 4.2), нужна ли аутентификация(п 5.1) - 15 мин

5.10 Проводим рефакторинг, чтобы использовать ArgHandler - 10 мин

### 6.Сценарий авторизации - 69 мин

6.1 Создаем перечисление для хранения ролей Write, Read, Execute (R1.5) - 5 мин 

6.2 Создаем метод нужна ли авторизация в классе `ArgHandler args[4] == "-res"` - 10 мин

6.3 Создаем метод который проверяет существует ли такой ресурс (проверяем правильность ресурса через regexp) - 7 мин

6.4 Создаем метод который проверяет существует ли доступ данного пользователя к данному ресурсу с данной ролью (тест: login == vasya, pass == 123, role == READ, res == A ) - 10 мин

6.5 Создаем data class Res(user, role, path) - 10 мин

6.6 Создаем метод предоставления такого же доступа пользователю к ресурсам-потомкам (R1.6) - 6 мин

6.7 Создаем список объектов Res (R1.4) - 5 мин

6.8 Модифицируем методы на работу с коллекцией - 10 мин

6.9 Модифицируем класс ArgHandler на работу с параметрами res и role - 10 мин

### 7.Сценарий аккаутинга - 80 мин

7.1 Создаем метод нужен ли аккаутинг в классе `ArgHandler args[6] == "-ds"` - 5 мин

7.2 Создаем метод который проверяет правильность ввода дат(ds < de + правильность формата) - 8 мин

7.3 Создаем метод который проверяет правильность ввода объема использования (vol > 0) - 7 мин

7.4 Создаем `data class Using(login, res, ds, de, vol)` (R1.7) - 15 мин

7.5 Создаем коллекцию объектов Using - 5 мин

7.6 Модифицируем методы для работы с коллекцией - 20 мин

7.7 Модифицируем класс ArgHandler на работу с параметрами ds, de и vol - 20 мин

### 8.Добавить к созданным методам необходимые коды (R1.9) - 35 мин

| Код| Значение  |
|:--:| :-------------: |
| 0 | Успех    |
| 1  | Справка    |
| 2  | Неверный формат логина |
| 3  | Неизвестный логин  |
| 4  | Неверный пароль    |
| 5  | Неизвестная роль  | 
| 6  | Нет доступа  | 
| 7  | Некорректная активность  |

### 9 Добавить хеширование пароля (R1.2) - 85 мин

9.1 Добавить генератор псевдо-случайных чисел для генерации соли - 15 мин

9.2 Добавить одну из хэш-функций, например SHA256 - 15 мин

9.3 Добавить в класс пользователя поле salt - 5 мин

9.4 Модифицировать методы проверки пароля - 20 мин

9.5 Сгенерировать для тестовых пользователей соль и захешировать пароли - 30 мин

### 10.Модифицируем методы класса ArgHandler для считывания аргументов в любом порядке используя kotlin.cli (R1.10) - 40 мин

| №| Предполагаемое время | Фактическое время |
|:--:| :-------------: |:------------------:| 
|1|	5||		
|2|	20||	
|2.1|	10||	
|2.2|	10||	
|3|	30||		
|4|	20||	
|4.1|	10||	
|4.2|	10||	
|4.3|	5||	
|4.4|	5||	
|5|	88||	
|5.1|	5||	
|5.2|	5||	
|5.3|	5||	
|5.4|	5||	
|5.5|	5||
|5.6|	10||	
|5.7|	20||	
|5.8|	8||	
|5.9|	15||	
|6|	69||	
|6.1|	5||	
|6.2|	10||	
|6.3|	7||	
|6.4|	10||	
|6.5|	10||	
|6.6|	6||
|6.7|	5||	
|6.8|	10||	
|6.9|	10||	
|7|	80||	
|7.1|	5||	
|7.2|	8||	
|7.3|	7||	
|7.4|	15||	
|7.5|	5||	
|7.6|	20||	
|7.7|	20||	
|8|	35||		
|9|85||	
|9.1|15||
|9.2|15||
|9.3|5||
|9.4|20||
|9.5|30||
|10| 40||
|всего |835||	



