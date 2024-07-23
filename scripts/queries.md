# Some queries are performed to answer specific questions.


## 1. Find the total milk production for the year 2023.
```
select sum(value) from milk_production where year is 2023;
```

## 2. Show coffee production data for the year 2015.
```
select sum(value) from coffee_production where year is 2015;
```

## 3. Find the average honey production for the year 2022.
```
select avg(value) from honey_production where year is 2022;
```

## 4. Get the state names with their corresponding ANSI codes from the state_lookup table. What number is Iowa?
```
select * from state_lookup where state is "IOWA";
```

## 5. Find the highest yogurt production value for the year 2022.
```
select max(value) from yogurt_production where year is 2022;
```

## 6. Find states where both honey and milk were produced in 2022. Did State_ANSI "35" produce both honey and milk in 2022?
```
select distinct h.state_ansi
from honey_production h
join milk_production m on h.state_ansi = m.state_ansi
where h.year = 2022 and m.year = 2022;
```

```
select 
    (select count(*) from honey_production where year = 2022 and state_ansi = 35) as honey_production_2022,
    (select count(*) from milk_production where year = 2022 and state_ansi = 35) as milk_production_2022;
```

## 7. Find the total yogurt production for states that also produced cheese in 2022.
```
select sum(y.value)
from yogurt_production y
where y.year = 2022 and y.state_ansi in (
    select distinct c.state_ansi from cheese_production c where c.year = 2022
);
```
