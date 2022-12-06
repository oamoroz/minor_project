# Интернет-магазин строительных материалов
![drawSQL-export-2022-12-06_16_52](https://user-images.githubusercontent.com/115698405/205959050-ed788fce-13ab-4927-a90c-fa73d1fd63d6.png)
# Запросы
## 1. Tоп 5 клиентов, которые потратили больше всего денег в магазине

```sql
SELECT c.id, concat_ws(' ', c.name, c.surname) as client, sum(o.sum) as total_amount
FROM clients c
INNER JOIN orders1 o on c.id=o.client_id
GROUP BY c.id, client
ORDER BY total_amount desc
LIMIT 5
```
# Результат
```
0 Record(id=7, client='Jungkook Jeon', total_amount=143146)
1 Record(id=2, client='Soekjin Kim', total_amount=89058)
2 Record(id=25, client='Conan Grey', total_amount=65937)
3 Record(id=16, client='Salma Hayek', total_amount=49278)
4 Record(id=6, client='Taehyung Kim', total_amount=46773)
```

## 2. Найти всех клиентов, которыe делали заказы в ноябре

```sql
SELECT c.id, concat_ws(' ', c.name, c.surname) as client, o.making_data
FROM clients c
INNER JOIN orders1 o on c.id=o.client_id
WHERE date_part ('month', o.making_data) = '11'
#WHERE o.making_data < '01.12.2022 00:00:00' AND o.making_data > '31.10.2022 23:59:59'
```
# Результат
```
0 Record(id=2, client='Soekjin Kim', making_data=datetime.datetime(2022, 11, 11, 13, 16, 38))
1 Record(id=4, client='Hoseok Jung', making_data=datetime.datetime(2022, 11, 26, 14, 22, 38))
2 Record(id=5, client='Jimin Park', making_data=datetime.datetime(2022, 11, 12, 14, 26, 38))
3 Record(id=6, client='Taehyung Kim', making_data=datetime.datetime(2022, 11, 29, 14, 18, 38))
4 Record(id=7, client='Jungkook Jeon', making_data=datetime.datetime(2022, 11, 10, 16, 5, 38))
5 Record(id=11, client='Whee In Jung', making_data=datetime.datetime(2022, 11, 10, 11, 45, 38))
6 Record(id=12, client='David Tennant', making_data=datetime.datetime(2022, 11, 9, 16, 9, 38))
7 Record(id=13, client='Michel Sheen', making_data=datetime.datetime(2022, 11, 22, 16, 47, 38))
8 Record(id=16, client='Salma Hayek', making_data=datetime.datetime(2022, 11, 9, 16, 35, 38))
9 Record(id=17, client='Helena Bonham Carter', making_data=datetime.datetime(2022, 11, 22, 15, 25, 38))
10 Record(id=18, client='Tilda Swinton', making_data=datetime.datetime(2022, 11, 17, 15, 8, 38))
11 Record(id=19, client='Mia Wasikowska', making_data=datetime.datetime(2022, 11, 19, 19, 35, 38))
12 Record(id=20, client='Chris Hemsworth', making_data=datetime.datetime(2022, 11, 28, 18, 15, 38))
13 Record(id=23, client='Hotaka Gate', making_data=datetime.datetime(2022, 11, 27, 16, 12, 38))
14 Record(id=24, client='Gaga Lady', making_data=datetime.datetime(2022, 11, 24, 17, 26, 38))
15 Record(id=24, client='Gaga Lady', making_data=datetime.datetime(2022, 11, 16, 17, 54, 38))
16 Record(id=25, client='Conan Grey', making_data=datetime.datetime(2022, 11, 3, 17, 36, 38))
17 Record(id=27, client='Atinius Rumford', making_data=datetime.datetime(2022, 11, 14, 18, 25, 38))
18 Record(id=28, client='Phrixus Ringleb', making_data=datetime.datetime(2022, 11, 30, 11, 55, 38))
19 Record(id=31, client='Annegreth Thornton', making_data=datetime.datetime(2022, 11, 18, 17, 6, 38))
20 Record(id=33, client='Justin Wheatstone', making_data=datetime.datetime(2022, 11, 25, 13, 54, 38))
21 Record(id=34, client='Hotaka Callaud', making_data=datetime.datetime(2022, 11, 15, 13, 46, 38))
22 Record(id=35, client='Karolina Oresme', making_data=datetime.datetime(2022, 11, 15, 11, 5, 38))
23 Record(id=36, client='Maeko Winckler', making_data=datetime.datetime(2022, 11, 22, 17, 38, 38))
```

## 3. Найти самую старую дату начисления бонусов

```sql
SELECT min(accrual_data)
FROM loyalty_program
```
# Результат
```
0 Record(min=datetime.datetime(2022, 7, 18, 12, 35, 38))
```

## 4. В какой день в октябре было сделано больше всего заказов

```sql
SELECT date_part('day', o.making_data) as day, count(o.id) as count
FROM orders1 o
WHERE date_part ('month', o.making_data) = '10'
GROUP BY day
ORDER BY count desc
LIMIT 1
```
# Результат
```
0 Record(day=9.0, count=3)
```

## 5. Топ популярных товаров для категории

```sql
SELECT cat.id, p.name, p.sold
FROM products p
INNER JOIN categories cat on p.category_id=cat.id
WHERE cat.id = 8
GROUP BY cat.id, p.name, p.sold
ORDER BY p.sold desc
```
# Результат
```
0 Record(id=8, name='cellular polycarbonate 8', sold=787)
1 Record(id=8, name='cellular polycarbonate 3', sold=398)
2 Record(id=8, name='watering hose', sold=250)
3 Record(id=8, name='lawn mower', sold=200)
4 Record(id=8, name='petrol mower', sold=108)
5 Record(id=8, name='reinforced hose', sold=76)
6 Record(id=8, name='watering device', sold=45)
7 Record(id=8, name='brush cutter', sold=12)
8 Record(id=8, name='motor pump', sold=0)
```

## 6. Kакие сотрудники приписаны к какому пункту выдачи

```sql
SELECT concat_ws(' ', ep.name, ep.surname) as employee, e.pickup_point_id, concat_ws(' ', pp.street, pp.house_number) as adress
FROM employees_personal ep
INNER JOIN employees e on ep.id=e.id
INNER JOIN pickup_points pp on e.pickup_point_id=pp.id
GROUP BY e.pickup_point_id, employee, adress
ORDER BY e.pickup_point_id
```
# Результат
```
0 Record(employee='Domitius Hardy', pickup_point_id=1, adress='Lomonosovskaya 5')
1 Record(employee='Yogi Waller', pickup_point_id=1, adress='Lomonosovskaya 5')
2 Record(employee='Mitchell Thompson', pickup_point_id=2, adress='Moskovsky 63')
3 Record(employee='Ralph Buchner', pickup_point_id=2, adress='Moskovsky 63')
4 Record(employee='Tina Becquerel', pickup_point_id=2, adress='Moskovsky 63')
5 Record(employee='Victor Pfeffer', pickup_point_id=2, adress='Moskovsky 63')
6 Record(employee='Baird Stewart', pickup_point_id=3, adress='Nevsky 144')
7 Record(employee='Cassidy Guinan', pickup_point_id=3, adress='Nevsky 144')
8 Record(employee='Constantius Rothschild', pickup_point_id=3, adress='Nevsky 144')
9 Record(employee='Tanya Knight', pickup_point_id=3, adress='Nevsky 144')
10 Record(employee='Katy Crelle', pickup_point_id=4, adress='Blagodatnaya 17')
11 Record(employee='Septimius Grenet', pickup_point_id=4, adress='Blagodatnaya 17')
12 Record(employee='Nami Campbel', pickup_point_id=5, adress='Sedova 36')
13 Record(employee='Sebasteia Sebasteia', pickup_point_id=5, adress='Sedova 36')
14 Record(employee='Timothy Hencky', pickup_point_id=5, adress='Sedova 36')
15 Record(employee='Amelie Whitney', pickup_point_id=6, adress='Chernachovskogo 8')
16 Record(employee='Cecilie Caratheodory', pickup_point_id=6, adress='Chernachovskogo 8')
17 Record(employee='Ole Cronkite', pickup_point_id=6, adress='Chernachovskogo 8')
18 Record(employee='Duff Kuster', pickup_point_id=7, adress='Gazovaya 10')
19 Record(employee='Mathias Feynman', pickup_point_id=7, adress='Gazovaya 10')
20 Record(employee='Servilius Voigt', pickup_point_id=7, adress='Gazovaya 10')
21 Record(employee='Marcus Lebesgue', pickup_point_id=8, adress='Ligovsky 68')
22 Record(employee='Risako Dunne', pickup_point_id=8, adress='Ligovsky 68')
23 Record(employee='Roka Sanchez', pickup_point_id=8, adress='Ligovsky 68')
24 Record(employee='Terese Tarski', pickup_point_id=8, adress='Ligovsky 68')
```
