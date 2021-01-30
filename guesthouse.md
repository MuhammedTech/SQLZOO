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
