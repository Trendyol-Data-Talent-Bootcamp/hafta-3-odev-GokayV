```SQL
-------------kendimizde önce dosyayı oluşturduk-----------
CREATE OR REPLACE TABLE `dsmbootcamp.gokay_varcok.semi_structured_hw`
as
SELECT * FROM `dsmbootcamp.sample.semi_structured_hw`;

--------------içinde neler varmış görelim----------------
select * from `dsmbootcamp.gokay_varcok.semi_structured_hw`

-----------------------çözüm--------------------------
#name kolonunu seçmek
select name, car,manufacture.year as manufacture_year,
        array(select as struct
                p.id,
                TIMESTAMP_ADD(p.date, INTERVAL 3 HOUR) as date
                from unnest(purchase) as p
              ) purchase 
from `dsmbootcamp.gokay_varcok.semi_structured_hw`
#car ve manufacture arraylerini unnest ederek her iki array içindeki id kolonları üzerinden joinlemek
cross join unnest(car) car
cross join unnest(manufacture) manufacture
#Unnest ettiğimiz bu 2 array içinden car.id, car.model ve manufacture.year alanlarını uygun aliaslar ile seçmek
on car.id=manufacture.id
---------------Sıralama ----------
order by name;

--------------kontrol---------
(select * from `dsmbootcamp.sample.semi_structured_hw_expected`)
#aynı sonuçları verdi

```
