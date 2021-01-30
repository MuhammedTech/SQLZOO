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

## 6. List the Company name and the number of calls for those companies with more than 18 calls.

```SQL 
select c.company_name, count(*) as cc
from Customer c
join Caller ca on c.company_ref = ca.company_ref
join Issue i on ca.caller_id = i.caller_id
group by c.company_name
having count(*) > 18
```
## 7. Find the callers who have never made a call. Show first name and last name
```SQL 
select c.first_name, c.last_name
from Caller c
left join Issue i
on c.caller_id = i.caller_id
where i.call_date is null
```

## 8.  For each customer show: Company name, contact name, number of calls where the number of calls is fewer than 5

```SQL 
select cu.company_name,ca2.first_name,ca2.last_name,count(*) as nc
from Customer cu
inner join Caller ca on ca.company_ref = cu.company_ref 
inner join Issue i on i.caller_id = ca.caller_id
inner join Caller ca2 on ca2.caller_id = cu.contact_id
group by cu.company_name,ca2.first_name,ca2.last_name
having nc < 5
```

# 9.  For each shift show the number of staff assigned. Beware that some roles may be NULL and that the same person might have been assigned to multiple roles (The roles are 'Manager', 'Operator', 'Engineer1', 'Engineer2').
```SQL 
select a.shift_date, a.shift_type, count(Distinct role) as cw
from (
       select shift_date, shift_type, manager as role 
       from Shift
union all
       select shift_date, shift_type, operator as role 
       from Shift
union all
       select shift_date, shift_type, engineer1 as role 
       from Shift
union all
       select shift_date, shift_type, engineer2 as role 
       from Shift

     ) 
as a
group by a.shift_date, a.shift_type
```

## 10.  Caller 'Harry' claims that the operator who took his most recent call was abusive and insulting. Find out who took the call (full name) and when.
```SQL 
select s.first_name, s.last_name, i.call_date
from Staff s 
join Issue i on s.staff_code = i.taken_by
join Caller c i.caller_id = c.caller_id 
where c.first_name = 'Harry'
order by i.call_date desc limit 1
```

