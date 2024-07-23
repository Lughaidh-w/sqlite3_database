# Second part of queries

## 1. Can you find out the total milk production for 2023? Your manager wants this information for the yearly report. What is the total milk production for 2023?
```
select sum(value) from milk_production where year is 2023;
```

## 2. Which states had cheese production greater than 100 million in April 2023? The Cheese Department wants to focus their marketing efforts there. How many states are there?
```
select * from cheese_production where value > 100000000 and year is 2023 and period is "APR";
```

## 3. Your manager wants to know how coffee production has changed over the years. What is the total value of coffee production for 2011?
```
select sum(value) from coffee_production where year is 2011;
```

## 4. There's a meeting with the Honey Council next week. Find the average honey production for 2022 so you're prepared.
```
select avg(value) from honey_production where year is 2022;
```

## 5. What is the State_ANSI code for Florida?
```
select * from state_lookup where state is "FLORIDA";
```

## 6. What is the total cheese production for NEW JERSEY in April 2023?
```
select * from cheese_production where year is 2023 and period is "APR" and State_ANSI is 34;
```

## 7. Can you find the total yogurt production for states in the year 2022 which also have cheese production data from 2023?
```
select sum(y.value)
from yogurt_production y
where y.year = 2022 and y.state_ansi in (
    select distinct c.state_ansi from cheese_production c where c.year = 2023
);
```

## 8. List all states from state_lookup that are missing from milk_production in 2023. How many states are there?
```
select s.state
from state_lookup s
where s.state_ansi not in (
    select distinct m.state_ansi from milk_production m where m.year = 2023
);
```

```
select count(*)
from state_lookup s
where s.state_ansi not in (
    select distinct m.state_ansi from milk_production m where m.year = 2023
);
```

## 9. List all states with their cheese production values, including states that didn't produce any cheese in April 2023.
```
select s.state, ifnull(c.value, 0) as cheese_production
from state_lookup s
left join cheese_production c on s.state_ansi = c.state_ansi and c.year = 2023 and c.period = 'APR';
```

## 10. Find the average coffee production for all years where the honey production exceeded 1 million.
```
select avg(c.value) as average_coffee_production
from coffee_production c
where c.year in (
    select h.year
    from honey_production h
    group by h.year
    having sum(h.value) > 1000000
);
```
