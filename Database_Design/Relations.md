# Mapping

## Customer

| column    | type                   |
| --------- | ---------------------- |
| <u>ID</u> | int, unique            |
| Age       | int, Computed from Age |
| BirthDate | Date                   |
| Password  | nvarchar(100)          |
| FirstName | nvarchar(25)           |
| LastName  | nvarchar(25)           |
| Email     | nvarchar(50)           |
| phone     | nvarchar(15)           |
| Address   | nvarchar(40)           |

=> <mark style="background: #FFB8EBA6;">Relations</mark>

1. Customer send messages ( 1 to N): Done
2. customer has chat ( 1 to 1 ): Done
3. customer Receive Purchase ( 1 to N): Done
4. Customer Has ShoppingCart (1 to 1): Done
5. Customer Make Review (1 to N): Done
6. Customer upload prescription (1 to N): Done
7. Customer Take Order ( 1 to N): Done



**Customer_Address**

| Column     | type         |
| ---------- | ------------ |
| <u>ID</u>  | PK: int      |
| CustomerID | FK: int      |
| street     | smallint     |
| Country    | nvarchar(50) |
| City       | nvarchar(50) |

-----

## Review

| column     | type     |
| ---------- | -------- |
| Id         | PK:int   |
| CustomerId | FK: int  |
| ProductId  | FK: int  |
| ReviewData | Date     |
| Rating     | tinyint  |
| Comment    | nvarchar |
=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. customer Make Review (1 to N): Done
2. Product Has Review (1 to N):Done

-------------
## ShoppingCart

| Column     | type    |
| ---------- | ------- |
| Id         | PK: Int |
| CustomerId | FK: int |
| CreatedAt  | Date    |
=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. shopping Cart Has Products (N to N): Done
2. Customer has only one Cart (1 to 1): Done


**Cart_Products**

| Column    | type    |
| --------- | ------- |
| ProductId | FK: int |
| CartId    | FK: int |


----
## Product

| Column               | type          |
| -------------------- | ------------- |
| Id                   | PK: int       |
| InventoryId          | FK: int       |
| Price                | float         |
| Name                 | nvarchar(100) |
| Expirydate           | Date          |
| QuantityinStock      | int           |
| BulkAllowed          | bit           |
| PrescriptionRequired | bit           |
| Barcode              | VARCHAR       |
| Image                | nvarchar(150) |

=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. Products are in shopping Carts ( N to N): Done
2. products are in orders ( N to N): Done
3. Product has Review ( 1 to N): Done
4. Product into category (N to M): Done
5. Products are in Inventory (1 to N): Done

**Order_Products**

| Column    | type    |
| --------- | ------- |
| ProductId | FK: int |
| OrderId   | FK: int |

**Product_Categories**

| Column     | type    |
| ---------- | ------- |
| ProductId  | FK: int |
| CategoryId | FK: int |


---------
## Category

| Column       |               |
| ------------ | ------------- |
| Id           | PK: smallInt  |
| CategoryName | nvarchar(100) |
=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. Category has products (N to M): Done

-----
## Purchase

| Column        | type     |
| ------------- | -------- |
| Id            | PK : int |
| OrderId       | FK: int  |
| CustomerId    | FK: int  |
| PharmacyId    | FK: int  |
| PaymentId     | FK: int  |
| PaymentStatus | Enum     |
| InvoiceNumber | BigInt   |
| TotalCost     | float    |
| isVendor      | bit      |
=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. Purchase has received to customer (1 to N): Done
2. purchase belongs to order ( 1 to 1): Done
3. Purchase has received to pharmacy (1 to N): Done
4. Purchase is delivered from pharmacy (1 to N): Done
	1. if `isVendor` true, so `CustomerId` is another `pharamcyId`
5. purchase contains Payment (1 to 1):Done

-------

## Payment

| Column | type    |
| ------ | ------- |
| Id     | PK: Int |
| Type   | Enum    |
| Status | Enum    |
=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. purchase contains Payment (1 to 1): done

-----
## Pharmacy

| Column    | type          |
| --------- | ------------- |
| Id        | PK:int        |
| ManagerId | FK: int       |
| Location  | nvarchar(150) |
| Name      | nvarchar(150) |

=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. pharmacy deliver purchase ( 1 to N): Done
2. pharmacy Receive purchase (1 to N):Done
3. Pharmacy has inventory (1 to 1): Done
4. pharmacy managed by one pharmacist (1 to 1): Done
5. pharmacy has pharmacists (1 to N): Done

-----
## Inventory

| Column        | type          |
| ------------- | ------------- |
| Id            | PK: int       |
| PharmacyId    | FK: int       |
| QuantityStock | int           |
| Location      | nvarchar(150) |

=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. Inventory related to one pharmacy ( 1 to 1): Done
2. Inventory send alert about stock (1 to N): Done
3. Inventory has products (1 to N): Done

---
## Notification

| Column     | type     |
| ---------- | -------- |
| Id         | PK: int  |
| SenderId   | FK: int  |
| SenderType | ENUM     |
| Message    | nvarchar |
| IsRead     | bit      |
| CreatedAt  | Date     |

=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. Notification is send by inventory ( 1 to N): Done
2. Notification is send to confirm order ( 1 to N): Done

------
## Order

| Column          | type     |
| --------------- | -------- |
| Id              | PK: int  |
| PharmacistId    | FK: int  |
| CustomerId      | FK: int  |
| PharmacyId      | FK: int  |
| OrderType       | ENUM     |
| OrderStatus     | ENUM     |
| DeliveryAddress | nvarchar |
| TotalPrice      | float    |

=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. order and customer ( 1 to N): Done
2. Order `belongTo` purchase (1 to 1) : Done
3. order and products ( N to N ): Done
4. order and pharmacist ( 1 to N ): Done
5. order and Notification (1 to N ): Done
6. order and pharmacy ( 1 to N): Done

-----
## Pharmacist

| column     | type                   |
| ---------- | ---------------------- |
| <u>ID</u>  | PK: int, unique        |
| PharmacyId | FK: int                |
| Age        | int, Computed from Age |
| BirthDate  | Date                   |
| Password   | nvarchar(100)          |
| FirstName  | nvarchar(25)           |
| LastNamem  | nvarchar(25)           |
| Email      | nvarchar(50)           |
| phone      | nvarchar(15)           |
| HireDate   | Date                   |
| Address    | nvarchar(40)           |

=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. pharmacist send messages ( 1 to N): Done
2. pharmacist Access Chat (N to N): Done
3. pharmacist Review Prescription (1 to N): Done
4. pharmacist process Order ( 1 to N): Done
5. pharmacist is manager ( 1 to 1): Done
6. pharmacist work in pharmacy (1 to N): Done


**Pharmacist_Chat**

| Column       | type    |
| ------------ | ------- |
| pharmacistId | FK: int |
| ChatId       | FK: int |


---
## Prescription


| column       | type    |
| ------------ | ------- |
| Id           | PK: int |
| CustomerId   | FK: int |
| pharmacistId | FK: int |
| UploadDate   | Date    |
| Status       | ENUM    |
=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. prescription and customer ( 1 to N): Done
2. prescription and pharmacist ( 1 to N): Done

---------
## Chat

| Column     | type    |
| ---------- | ------- |
| Id         | PK: int |
| CustomerId | FK: int |
=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. chat and customer (1 to 1 ): Done 
2. chat and pharmacist ( N to N ): Done
3. chat and messages ( 1 to N ): Done
---
## Message

| columna    | type     |
| ---------- | -------- |
| Id         | PK: int  |
| ChatId     | FK: int  |
| SenderID   | FK: int  |
| SenderType | ENUM     |
| Message    | nvarchar |
=> <mark style="background: #FFB8EBA6;">Relation</mark>
1. chat and messages ( 1 to N ): Done
2. messages and customer (1 to N ): Done
3. Message and pharmacist ( 1 to N ): Done

`SenderId`=> depend on senderType Column
	1. if customer -> so it will be Customer Id
	2. if pharamcist -> so it will be pharmacist Id

----------
**notes about ERD:**
1. supply relation between purchase and customer => deliver
2. Creating pharmacy table and link it with (purchased, inventory, pharmacist)
3. make review strong
4. move `QuantityPurchased`, `Total Cost` to `Purchased Table`
5. remove `InventoryId`, `uniCost` and `ExpiryDate` (they already in Medicine table)
6. remove relation between payment and notification
7. remove relation between payment and order
8. make relation between purchased and payment
9. remove relation between medicine and purchase
10. remove relation of pharmacist and inventory and put relation between pharmacy and pharmacist
