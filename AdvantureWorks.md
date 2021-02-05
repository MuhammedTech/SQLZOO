Link to the challange https://sqlzoo.net/wiki/Help_Desk

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