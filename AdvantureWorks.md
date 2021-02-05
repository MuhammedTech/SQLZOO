Link to the challange https://sqlzoo.net/wiki/AdventureWorks

## 1. Show the CompanyName for James D. Kramer

```SQL 
select CompanyName
from Customer
where FirstName = 'James' and MiddleName = 'D.' and LastName = 'Kramer'
```

## 2. Show all the addresses listed for 'Modular Cycle Systems'

```SQL 
select c.CompanyName, ca.AddressType, a.AddressLine1
from Customer c
join CustomerAddress ca
on c.CustomerID = ca.CustomerID
join Address a
on a.AddressID = ca.AddressID
where CompanyName = 'Modular Cycle Systems'
```

## 3. Show OrdeQty, the Name and the ListPrice of the order made by CustomerID 635

```SQL
select sod.OrderQty, p.Name, p.ListPrice
from Product p
join SalesOrderDetail sod
on p.ProductID = sod.ProductID
join SalesOrderHeader soh
on soh.SalesOrderID = sod.SalesOrderID
join Customer c
on c.CustomerID = soh.CustomerID
where soh.CustomerID = 635
```

/* Resit Questions */

## 4. List the SalesOrderNumber for the customer 'Good Toys' 'Bike World'
```SQL
select SalessOrderNumber
from SalesOrderHeader
join Customer
on Customer.CustomerID = SalesOrderHeader.CustomerID
where CompanyName in ('Good Toys', 'Bike World')
```
## 5. List the ProductName and the quantity of what was ordered by 'Futuristic Bikes'
```SQL
select
  p.Name,
  sod.OrderQty
from Customer c
join SalesOrderHeader soh
on c.CustomerID = soh.CustomerID
join SalesOrderDetail sod
on soh.SalesOrderID = sod.SalesOrderID
join Product p
on sod.ProductID = p.ProductID
where c.CompanyName = 'Futuristic Bikes';
```


## 6. List the name and addresses of companies containing the word 'Bike' (upper or lower case) and companies containing 'cycle' (upper or lower case). Ensure that the 'bike's are listed before the 'cycles's.
```SQL
select c.CompanyName, a.AddressLine1
from Customer c
join CustomerAddress ca
on ca.CustomerID = c.CustomerID
join Address a
on a.AddressID = ca.AddressID
where CompanyName regexp 'Bike | cycle'
order by CompanyName like  '%Bike%' desc
```

## 7. Show the total order value for each CountryRegion. List by value with the highest first.
```SQL
select a.CountyRegion, sum(soh.SubTotal)
from Address a
join SalesOrderHeader soh
on soh.BillToAddressID = a.AddressID
group by a.CountyRegion
order by soh.SubTotal
```


