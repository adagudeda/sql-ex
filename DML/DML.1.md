## 1
Добавить в таблицу PC следующую модель:
    code: 20
    model: 2111
    speed: 950
    ram: 512
    hd: 60
    cd: 52x
    price: 1100
```sql
insert into pc
values(20, '2111', 950, 512, 60, '52x', 1100);
```
## 2
Добавить в таблицу Product следующие продукты производителя Z:
принтер модели 4003, ПК модели 4001 и блокнот модели 4002

```sql
insert into product values
('Z', '4003', 'Printer'),
('Z', '4001', 'PC'),
('Z', '4002', 'Laptop')
```

## 3
Добавить в таблицу PC модель 4444 с кодом 22, имеющую скорость процессора 1200 и цену 1350.
Отсутствующие характеристики должны быть восполнены значениями по умолчанию, принятыми для соответствующих столбцов.

```sql
insert into pc (model, code, speed, price)
values ('4444', 22, 1200, 1350)
```
## 4
Для каждой группы блокнотов с одинаковым номером модели добавить запись в таблицу PC со следующими характеристиками:
код: минимальный код блокнота в группе +20;
модель: номер модели блокнота +1000;
скорость: максимальная скорость блокнота в группе;
ram: максимальный объем ram блокнота в группе *2;
hd: максимальный объем hd блокнота в группе *2;
cd: значение по умолчанию;
цена: максимальная цена блокнота в группе, уменьшенная в 1,5 раза.
Замечание. Считать номер модели числом. 

```sql
insert into PC
(code, model, speed, ram, hd, price)
select
    min(code)+20 code
  , cast(model as int)+1000 as model
  , max(speed) speed
  , max(ram)*2 ram
  , max(hd)*2 hd
  , max(price)/1.5 price
  from laptop
  group by model
```

## 5
Удалить из таблицы PC компьютеры, имеющие минимальный объем диска или памяти. 

```sql
delete from pc
where hd = (select min(hd) from pc)
      OR ram = (select min(ram) from pc)
```

## 6
Удалить все блокноты, выпускаемые производителями, которые не выпускают принтеры.

```sql
delete from laptop
where model in (
  select model from product
  where maker not in (select maker from product where type='Printer')
)
```

## 7
Производство принтеров производитель A передал производителю Z. Выполнить соответствующее изменение

```sql
update product
set maker='Z' where (maker='A' and type='Printer');
```

## 8
Удалите из таблицы Ships все корабли, потопленные в сражениях.

```sql
delete from Ships
where name in (
  select ship from outcomes
  where result='sunk'
);
```

## 9
Измените данные в таблице Classes так, чтобы калибры орудий измерялись в сантиметрах (1 дюйм=2,5см), 
а водоизмещение в метрических тоннах (1 метрическая тонна = 1,1 тонны). Водоизмещение вычислить с точностью доцелых.

```sql
update classes
set bore=(bore*2.5)
    , displacement=ROUND(displacement*1.0/1.1, 0);
```
