Link to the challange https://sqlzoo.net/wiki/Help_Desk

## 1. There are three issues that include the words "index" and "Oracle". Find the call_date for each of them

```SQL 
select DATE_FORMAT(call_date, '%Y-%m-%d %T.%f'), call_ref
from Issue
where Detail like '%index%' and Detail like '%Oracle%'
```

## 2. Samantha Hall made three calls on 2017-08-14. Show the date and time for each

```SQL
select DATE_FORMAT(i.call_date, '%Y-%m-%d %T'), c.first_name, c.last_name
from Issue i
join Caller c
on i.Caller_id = c.Caller_id
where c.first_name = 'Samantha' and c.last_name = 'Hall' and i.call_date like '2017-08-14%'
```

## 3.There are 500 calls in the system (roughly). Write a query that shows the number that have each status.

```SQL
select status, count(status) as Volume
from Issue
group by status
```

## 4. Calls are not normally assigned to a manager but it does happen. How many calls have been assigned to staff who are at Manager Level?

```SQL
select count(i.assigned_to) as mlcc
from Issue i
join Staff s
on s.staff_code = i.assigned_to
join Level l
on s.level_code = l.level_code
where l.manager = 'Y'
```


## 5.Show the manager for each shift. Your output should include the shift date and type; also the first and last name of the manager.


```SQL
select s.Shift_date, s.Shift_type, st.first_name, st.last_name
from Shift s
join Staff st
on s.manager = st.staff_code
```