## Шакирова Арина Васильевна, группа ИС22/9-1

### 2.
База данных спортзала, включает в себя 5 таблиц:
1)  clients (данные о клиентах);
2) coachs (данные о тренерах);
3) gum_class (номера спортзалов);
4) season_tickets (абонименты в спортзал и характеристики);
5) uchet (учет абонименетов по месяцам).
### 2.1
   CLEINTS. Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) last_name (фамилия, text)
3) name (имя, text)
4) phone_number (номер телефона, int)
5) id_coach (айди таблицы coachs, int)
6) id_uchet (айди таблицы uchet, int)
   ![](scrin/clients(a).png)
   ![](scrin/clients(b).png)

    COACHS. Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) last_name (фамилия, text)
3) phone_number (номер телефона, int)
![](scrin/coachs(a).png)
![](scrin/coachs(b).png)

    GYM_CLASSES.Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) number (номер спортзала, int)
![](scrin/gym_classes(a).png)
![](scrin/gym_classes(b).png)

    SEASON_TICKETS. Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) description (описание абонимента, text)
3) price (цена аюониментов, int)
4) date_s (дата начала срока действия абонемента, date)
5) amount (количество абониментов, int)
6) name_of_season_ticket (названия абониментов, text)
7) id_gym_class (айди таблицы gym_classes, int)
![](scrin/season_tickets(a).png)
![](scrin/season_tickets(b).png)

   UCHET. Имеет стобцы:
1) id (айдишник таблицы, настроен по умолчанию, int)
2) month (название месяца, text)
3) payment (оплата по абонементу, int)
4) id_season_ticket (айди таблицы gym_class, int)
![](scrin/uchet(a).png)
![](scrin/uchet(b).png)



