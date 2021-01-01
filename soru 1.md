```SQL
-------------kendimizde önce dosyayı oluşturduk-----------
CREATE OR REPLACE TABLE `dsmbootcamp.gokay_varcok.semi_structured_hw`
as
SELECT * FROM `dsmbootcamp.sample.semi_structured_hw`;
--------------içinde neler varmış görelim----------------
select * from `dsmbootcamp.gokay_varcok.semi_structured_hw`
-----------------------çözüm--------------------------

#name kolonunu seçmek
select name, car, manufacture.year as manufacture_year,
        struct(
                p.id,
                TIMESTAMP_ADD(p.date, INTERVAL 3 HOUR) as date
              ) purchase
              
from `dsmbootcamp.gokay_varcok.semi_structured_hw` gv

#car ve manufacture arraylerini unnest ederek her iki array içindeki id kolonları üzerinden joinlemek
cross join unnest(car) car
cross join unnest(manufacture) manufacture
cross join unnest(purchase) as p

#Unnest ettiğimiz bu 2 array içinden car.id, car.model ve manufacture.year alanlarını uygun aliaslar ile seçmek
on car.id=manufacture.id;

---------------Problemi geri array yapamadım işlem oldu ama hepsini çoklamış şekilde bıraktım .d ----------

```
