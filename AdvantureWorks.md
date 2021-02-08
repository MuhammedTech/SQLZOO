Link to the challange https://sqlzoo.net/wiki/AdventureWorks

/* Sample Queries */

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

## 3. Show OrdeQty, the Name and the ListPrice of the order made by CustomerID 635.

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

/* Easy to Hard */

## 1. Show the first name and the email address of customer with CompanyName 'Bike World'

```SQL 
select firstname, emailaddress
from Customer
where companyname = 'Bike World'
```

## 2. Show the CompanyName for all customers with an address in City 'Dallas'. 

```SQL 
select companyname
from Customer c
join CustomerAddress ca on ca.CustomerID = c.CustomerID
join Address a on a.AddressId = ca.AddressID
where a.City = 'Dallas'
```

## 3. How many items with ListPrice more than $1000 have been sold? 

```SQL 
select count(*) as Total
from SalesOrderDetail sod
join Product p on p.ProductID = sod.ProductID
where p.listprice > 1000
```

## 4. Give the CompanyName of those customers with orders over $100000. Include the subtotal plus tax plus freight.

```SQL 
select c.companyname, sum(soh.SubTotal + soh.TaxAmt + soh.Freight)
from Customer c
join SalesOrderHeader soh 
on c.CustomerID = soh.CustomerID
group by c.companyname
having sum(soh.SubTotal + soh.TaxAmt + soh.Freight) > 100000;
```

## 5. Find the number of left racing socks ('Racing Socks, L') ordered by CompanyName 'Riding Cycles'

```SQL 
select sum(sod.orderqty)
from Product p
join SalesOrderDetail sod on sod.productid = p.productid
join SalesOrderHeader soh on soh.salesorderid = sod.salesorderid
join Customer c on c.customerid = soh.customerid
where p.name = 'Racing Socks, L' and c.Companyname = 'Riding Cycles'

```


## 6. A "Single Item Order" is a customer order where only one item is ordered. Show the SalesOrderID and the UnitPrice for every Single Item Order.

```SQL
select sod.SalesOrderID, sod.UnitPrice
from SalesOrderDetail sod
join(
select soh.SalesOrderID
from SalesOrderDetail soh
group by soh.SalesOrderID
having count(soh.SalesOrderID) = 1) sodd
on sodd.SalesOrderID = sod.SalesOrderID
```

## 7. Where did the racing socks go? List the product name and the CompanyName for all Customers who ordered ProductModel 'Racing Socks'.
```SQL
with product_name as 
(select SalesOrderDetail.SalesOrderID, Product.Name
from Product
join SalesOrderDetail
on Product.ProductID = SalesOrderDetail.ProductID
join ProductModel
on Product.ProductModelID = ProductModel.ProductModelID
where ProductModel.name = 'Racing Socks')

select Name, sec.CompanyName
from product_name
join  
(select soh.SalesOrderID, ca.CompanyName 
from SalesOrderHeader soh
join Customer ca
on soh.CustomerID = ca.CustomerID) as sec
on sec.SalesOrderId = product_name.SalesOrderID
```

## 8. Show the product description for culture 'fr' for product with ProductID 736.
```SQL
with prod_desc as 

(select pd.description, pmpd.productmodelid
from ProductDescription pd
join ProductModelProductDescription pmpd
on pmpd.productdescriptionid = pd.productdescriptionid
where pmpd.culture = 'fr')

select prod_desc.description
from prod_desc
join 
(select pr.productmodelid, p.productid 
from Product p 
join ProductModel pr 
on pr.productmodelid = p.productmodelid 
where p.productid = 736) sec
on sec.productmodelid = prod_desc.productmodelid
```

## 9. Use the SubTotal value in SaleOrderHeader to list orders from the largest to the smallest. For each order show the CompanyName and the SubTotal and the total weight of the order.
```SQL
with NameTotal as 
(select soh.SalesOrderID,c.CompanyName, soh.Subtotal
from Customer c
join SalesOrderHeader soh
on soh.CustomerID = c.CustomerID)

select sod.SalesOrderID ,nt.CompanyName, sum(p.weight), sum(nt.Subtotal)
from Product p
join SalesOrderDetail sod
on sod.productID = p.ProductID
join NameTotal nt
on nt.SalesOrderID = sod.SalesOrderID
group by sod.SalesOrderID ,nt.CompanyName
order by nt.Subtotal desc
```

## 10. How many products in ProductCategory 'Cranksets' have been sold to an address in 'London'?
```SQL
with sod as (select sod.SalesOrderID, sod.OrderQty
from ProductCategory pc
join Product p on pc.productcategoryid = p.productcategoryid
join SalesOrderDetail sod on sod.ProductID = p.ProductID
where pc.Name = 'Cranksets')

select sum(sod.OrderQty)
from sod
join SalesOrderHeader soh on soh.SalesOrderID = sod.SalesOrderID
join Address a on a.AddressID = soh.BillToAddressID
where a.City = 'London'
```




/* Resit Questions */

## 1. List the SalesOrderNumber for the customer 'Good Toys' 'Bike World'
```SQL
select SalessOrderNumber
from SalesOrderHeader
join Customer
on Customer.CustomerID = SalesOrderHeader.CustomerID
where CompanyName in ('Good Toys', 'Bike World')
```
## 2. List the ProductName and the quantity of what was ordered by 'Futuristic Bikes'
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


## 3. List the name and addresses of companies containing the word 'Bike' (upper or lower case) and companies containing 'cycle' (upper or lower case). Ensure that the 'bike's are listed before the 'cycles's.
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

## 4. Show the total order value for each CountryRegion. List by value with the highest first.
```SQL
select a.CountyRegion, sum(soh.SubTotal)
from Address a
join SalesOrderHeader soh
on soh.BillToAddressID = a.AddressID
group by a.CountyRegion
order by soh.SubTotal
```


