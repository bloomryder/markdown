                               # Шакирова Арина Васильевна, группа ИС22/9-1

## 2. Описание БД
База данных спортзала, включает в себя 5 таблиц:
1)  clients (данные о клиентах);
2) coachs (данные о тренерах);
3) gum_class (номера спортзалов);
4) season_tickets (абонименты в спортзал и характеристики);
5) uchet (учет абонименетов по месяцам).

Физичесткая бд (supobase)


![](scrin/bd.png)

## 2.1 Описание Таблиц
   CLEINTS. Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) last_name (фамилия, text)
3) name (имя, text)
4) phone_number (номер телефона, int)
5) id_coach (айди таблицы coachs, int)
6) id_uchet (айди таблицы uchet, int)


![](scrin/clients(a).png)
![](scrin/clients(b).png)

###    COACHS. Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) last_name (фамилия, text)
3) phone_number (номер телефона, int)


![](scrin/coachs(a).png)
![](scrin/coachs(b).png)

###    GYM_CLASSES.Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) number (номер спортзала, int)


![](scrin/gym_classes(a).png)
![](scrin/gym_classes(b).png)

###    SEASON_TICKETS. Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) description (описание абонимента, text)
3) price (цена аюониментов, int)
4) date_s (дата начала срока действия абонемента, date)
5) amount (количество абониментов, int)
6) name_of_season_ticket (названия абониментов, text)
7) id_gym_class (айди таблицы gym_classes, int)


![](scrin/season_tickets(a).png)
![](scrin/season_tickets(b).png)

###   UCHET. Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) month (название месяца, text)
3) payment (оплата по абонементу, int)
4) id_season_ticket (айди таблицы gym_class, int)


![](scrin/uchet(a).png)
![](scrin/uchet(b).png)

## 3. UNION
 Обьединение наборов строк
обьединил строки last_name из таблиц clients, coachs


![](operation/union.png)
```
   SELECT last_name
   FROM clients
   UNION 
   SELECT last_name
   FROM coachs
   ORDER BY last_name
```
### 4. ORDER BY
 Сортировка
Упорядочение по столбцу last_name


![](operation/orderby.png)
```
  select last_name, name, phone_number
  from clients
  order by last_name
```
### 5. HAVING 
 Фильтрация запросов
Отфильтровал таблицу по avg_price 

```
SELECT name_of_season_tickets, AVG(price) as avg_price
FROM season_tickets
GROUP BY name_of_season_tickets
HAVING avg_price > 15000
```

![](operation/having.png)

### 6. SELECT (Вложенный запрос)
 Вывод с вложенным запросом в WHERE 
Вывод результата с именем "артем"
```
SELECT last_name, name FROM clients
WHERE last_name = (SELECT last_name FROM clients
                   WHERE name = 'Artem')
```
![](operation/select1.png)

Вывод номера телефона, где имя равно "василий"
```
SELECT last_name, name, phone_number FROM clients
WHERE phone_number = (SELECT phone_number FROM clients
                      WHERE name = 'Vasiliy')
```

![](operation/select2.png)

вывод цены > 15.000 с описанием абонемента
```
SELECT description, price, amount FROM season_tickets
WHERE price = (SELECT price FROM season_tickets
               WHERE price = 15000)
```
![](operation/select3.png)

### 7.1. АГРЕГАТНЫЕ ФУНКЦИИ 
 Математические функции 

Вывод максимальной цены из таблицы учета
```
SELECT month, MAX(payment) AS Максимальная_цена
FROM uchet
```

![](operation/agr1.png)

Вывод самого длинного номера телефона и количество клиентов 
```
SELECT MAX(LENGTH(phone_number)) AS Самый_длинный_номер, 
	COUNT(id) as Количество_клиентов
FROM clients
```

![](operation/agr2.png)

Вывод количества абонементов и видов абонементов 
```
SELECT amount AS Количество_абонементов,
COUNT(name_of_season_tickets) AS Количество_видов_абонеметов
FROM season_tickets
```

![](operation/agr3.png)

### 7.2. РАНЖИРУЮЩИЕ ФУНКЦИИ
 Сортировка строк с одним столбцом, номер строк для нумерации.
Вывод стоблцов таблицы, возвращение ранга стобца price, возвращение нумерации 
```
SELECT id,
	description,
    price,
    date_s,
    RANK() OVER(PARTITION BY price ORDER BY description) AS 'rank',
    ROW_NUMBER() OVER(PARTITION BY price ORDER BY description) AS 'row_number'
FROM season_tickets
```

![](operation/ranzh.png)

### 7.3. ФУНКЦИИ СМЕЩЕНИЯ
 Перемещение строк относительно других
Обращение к другим данным столбца description, общение к другим данным из следующих строк,
возвращает первое значение в окне
```
SELECT id,
	description,
    price,
    date_s,
    LAG(price) OVER(PARTITION BY description ORDER BY price) AS 'lag',
    LEAD(price) OVER(PARTITION BY description ORDER BY price) AS 'lead',
    FIRST_VALUE(price) OVER(PARTITION BY description ORDER BY price) AS 'first_value'
from season_tickets
```

![](operation/smesh.png)


### 8. JOIN 
  inner - Обьединяет таблицы (у меня через id)
Вывод данных о тренерах и клиетах и их номера телефонов
(coachs, clients)
 ```
SELECT clients.last_name as Фамилия_клиента, clients.name as Имя_клиента,
coachs.last_name as Фамилия_тренера, coachs.name as Имя_тренера,
clients.phone_number as Номер_клиента, coachs.phone_number as Номер_тренера
FROM clients
JOIN coachs on clients.id_coach = coachs.id
```
![](operation/join1.png)

inner - Вывод данных о тренерах и их спортзалы(номера)
(coachs, gym_classes)
```
SELECT coachs.last_name AS Фамилия_тренера,
	coachs.name AS Имя_тренера,
    gym_classes.number AS Номер_спортзала
FROM coachs
JOIN gym_classes ON coachs.id = gym_classes.id
```

![](operation/join2.png)


inner - Вывод описания абонемента, цены, месяца действия, оплаты из таблицы учета
(season_tickets, uchet)

```
SELECT season_tickets.description, season_tickets.price, uchet.month, uchet.payment
FROM season_tickets
JOIN uchet ON season_tickets.id = uchet.id
```


![](operation/join3.png)

right - Вывод имени, фамилии клиентов и фамилиии и номера телефона тренеров
```
SELECT clients.last_name, clients.name, coachs.last_name, coachs.phone_number
FROM clients
RIGHT JOIN coachs ON clients.id = coachs.id
```


![](operation/rightjoin.png)

left - Вывод имени, фамилии клиентов и фамилиии и номера телефона тренеров
```
SELECT clients.last_name, clients.name, coachs.last_name, coachs.phone_number
FROM clients
LEFT JOIN coachs ON clients.id = coachs.id
```

![](operation/leftjoin.png)


### 9. CASE
 Работает как конструкция IF
Сопоставление номера телефона тренера и если он меньше 12 знаков, то это редкий номер
```
SELECT phone_number as Номер_Тренера,
CASE
  WHEN length(phone_number) > 12 THEN 'Редкий_Номер_телефона'
  ELSE 'Обычный_Номер'
END as Классификация_номера
FROM coachs
  ```

![](operation/case.png)


### 9. WITH
 Создание временной таблицы 
Таблица содержит имена тренеров и клиентов
```
WITH all_names AS 
	(SELECT clients.name AS Имя_клиента, coachs.name AS Имя_тренера FROM clients
     JOIN coachs
     ON clients.id_coach = coachs.id)
     
SELECT * FROM all_names
```
![](operation/with.png)






