# Интернет-магазин строительных материалов
![drawSQL-export-2022-12-06_16_52](https://user-images.githubusercontent.com/115698405/205959050-ed788fce-13ab-4927-a90c-fa73d1fd63d6.png)
# Запросы
## 1 Tоп 5 клиентов, которые потратили больше всего денег в магазине

'''sql
select 
c.id,
concat_ws(' ', c.name, c.surname) as client,
sum(o.sum) as total_amount
                  
from clients c
inner join orders1 o
on c.id=o.client_id
group by c.id, client
order by total_amount desc
limit 5
'''
