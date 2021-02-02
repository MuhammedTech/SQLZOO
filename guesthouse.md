Link to the challenge: https://sqlzoo.net/wiki/Guest_House

## 1. Guest 1183. Give the booking_date and the number of nights for guest 1183.
```SQL 
select DATE_FORMAT(booking_date, '%Y-%m-%d'), nights
from booking
where guest_id = '1183'
```

## 2. When do they get here? List the arrival time and the first and last names for all guests due to arrive on 2016-11-05, order the output by time of arrival.
```SQL 
select b.arrival_time, g.first_name, g.last_name
from booking b
join guest g
on b.guest_id = g.id
where b.booking_date = '2016-11-05'
order by b.arrival_time
```

## 3. Look up daily rates. Give the daily rate that should be paid for bookings with ids 5152, 5165, 5154 and 5295. Include booking id, room type, number of occupants and the amount.
```SQL 
select b.booking_id, b.room_type_requested, r.occupancy, r.amount
from rate r
join booking b
on b.occupants = r.occupancy and b.room_type_requested = r.room_type
where booking_id in (5152, 5165, 5154, 5295)
```

## 4. Who’s in 101? Find who is staying in room 101 on 2016-12-03, include first name, last name and address.
```SQL 
select g.first_name, g.last_name, g.address
from guest g
join booking b
on g.id = b.guest_id
where b.room_no = 101 and b.booking_date = '2016-12-03'
```

## 5. How many bookings, how many nights? For guests 1185 and 1270 show the number of bookings made and the total number of nights. Your output should include the guest id and the total number of bookings and the total number of nights.
```SQL 
select guest_id, count(nights), sum(nights)
from booking
where guest_id in (1185, 1270)
group by guest_id
```

## 6.Ruth Cadbury. Show the total amount payable by guest Ruth Cadbury for her room bookings. You should JOIN to the rate table using room_type_requested and occupants.
```SQL
select sum(b.nights * r.amount) as 'SUM(nights*amount)'
from rate r
join booking b on r.room_type  = b.room_type_requested and r.occupancy = b.occupants
join guest g on g.id = b.guest_id
where g.first_name = 'Ruth' and g.last_name = 'Cadbury'
```

## 7.Including Extras. Calculate the total bill for booking 5346 including extras.
```SQL 
select r.amount + e.amount as 'SUM(amount)'
from booking b

join rate r on r.room_type = b.room_type_requested and r.occupancy = b.occupants

join 
         (select booking_id,sum(amount) as amount
          from extra 
          where booking_id = 5346
          group by booking_id) as e
on b.booking_id = e.booking_id
where b.booking_id = 5346
```

## 8.Edinburgh Residents. For every guest who has the word “Edinburgh” in their address show the total number of nights booked. Be sure to include 0 for those guests who have never had a booking. Show last name, first name, address and number of nights. Order by last name then first name.
```SQL 
select g.last_name, g.first_name ,g.address,  sum(case when b.nights is null then 0 else b.nights end) as nights
from guest g
left join booking b
on b.guest_id = g.id 
where g.address like '%Edinburgh%'
group by g.first_name, g.last_name, g.address
order by g.last_name, g.first_name
```


## 9. How busy are we? For each day of the week beginning 2016-11-25 show the number of bookings starting that day. Be sure to show all the days of the week in the correct order.
```SQL 
select booking_date, count(arrival_time) as arrivals
from booking
where booking_date between ('2016-11-25') and ('2016-12-01')
group by booking_date
```

## 10. How many guests? Show the number of guests in the hotel on the night of 2016-11-21. Include all occupants who checked in that day but not those who checked out.
```SQL 
select sum(occupants)
from booking
WHERE booking_date + INTERVAL nights DAY > '2016-11-21' AND booking_date <= '2016-11-21';
```
