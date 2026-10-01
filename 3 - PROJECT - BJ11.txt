CREATE TABLE FashionCategories (
    CategoryID CHAR(5) PRIMARY KEY,
    CategoryName VARCHAR(50) NOT NULL,
    CategoryDescription VARCHAR(255) NOT NULL,
    CONSTRAINT FashionCategories_CategoryID_CK CHECK(REGEXP_LIKE(CategoryID, '^CA[0-9]{3}$'))
);

CREATE TABLE FashionItems (
    ItemID CHAR(5) PRIMARY KEY,
    ItemName VARCHAR(255) NOT NULL,
    ItemDescription VARCHAR(255) NOT NULL,
    ItemPurchasePrice INT NOT NULL,
    ItemSalesPrice INT NOT NULL,
    ItemStock INT NOT NULL,
    CategoryID CHAR(5) NOT NULL,
    CONSTRAINT FashionItems_ItemID_CK CHECK(REGEXP_LIKE(ItemID, '^FA[0-9]{3}$')),
    CONSTRAINT FashionItems_ItemName_CK CHECK(LENGTH(ItemName) > 3),
    CONSTRAINT FashionItems_CategoryID_FK FOREIGN KEY (CategoryID) REFERENCES FashionCategories(CategoryID)
);

CREATE TABLE Customer (
    CustomerID CHAR(5) PRIMARY KEY,
    CustomerName VARCHAR(50) NOT NULL,
    CustomerGender VARCHAR(10) NOT NULL,
    CustomerAddress VARCHAR(255) NOT NULL,
    CustomerEmail VARCHAR(50) NOT NULL,
    CustomerPhone VARCHAR(50) NOT NULL,
    CONSTRAINT Customer_CustomerID_CK CHECK(REGEXP_LIKE(CustomerID, '^CU[0-9]{3}$')),
    CONSTRAINT Customer_CustomerName_CK CHECK(LENGTH(CustomerName) > 2),
    CONSTRAINT Customer_CustomerGender_CK CHECK(CustomerGender IN ('Male','Female')),
    CONSTRAINT Customer_CustomerEmail_CK CHECK(REGEXP_LIKE(CustomerEmail, '@gmail.com$')),
    CONSTRAINT Customer_CustomerPhone_CK CHECK(REGEXP_LIKE(CustomerPhone, '^08'))
);

CREATE TABLE Designer (
    DesignerID CHAR(5) PRIMARY KEY,
    DesignerName VARCHAR(50) NOT NULL,
    DesignerGender VARCHAR(10) NOT NULL,
    DesignerAddress VARCHAR(255) NOT NULL,
    DesignerEmail VARCHAR(50) NOT NULL,
    DesignerPhone VARCHAR(50) NOT NULL,
    CONSTRAINT Designer_DesignerID_CK CHECK(REGEXP_LIKE(DesignerID, '^DE[0-9]{3}$')),
    CONSTRAINT Designer_DesignerName_CK CHECK(LENGTH(DesignerName) > 3),
    CONSTRAINT Designer_Designeremail_CK CHECK(REGEXP_LIKE(DesignerEmail, '@modesign.com$')),
    CONSTRAINT Designer_Designerphone_CK CHECK(REGEXP_LIKE(DesignerPhone, '^08')),
    CONSTRAINT Designer_Designergender_CK CHECK(DesignerGender IN ('Male','Female'))
);

CREATE TABLE Staff (
    StaffID CHAR(5) PRIMARY KEY, 
    StaffName VARCHAR(50) NOT NULL,
    StaffGender VARCHAR(10) NOT NULL,
    StaffAddress VARCHAR(255) NOT NULL,
    StaffEmail VARCHAR(50) NOT NULL,
    StaffPhone VARCHAR(50) NOT NULL,
    CONSTRAINT Staff_StaffID_CK CHECK(REGEXP_LIKE(StaffID, '^SM[0-9]{3}$')),
    CONSTRAINT Staff_StaffName_CK CHECK(LENGTH(StaffName) > 3),
    CONSTRAINT Staff_StaffGender_CK CHECK(StaffGender IN ('Male', 'Female')),
    CONSTRAINT Staff_StaffEmail_CK CHECK(REGEXP_LIKE(StaffEmail, '@mostaff.com$')),
    CONSTRAINT Staff_StaffPhone_CK CHECK(REGEXP_LIKE(StaffPhone, '^08'))
);

CREATE TABLE PurchaseTransaction (
    PurchaseTransactionID CHAR(5) PRIMARY KEY,
    StaffID CHAR(5) NOT NULL,
    DesignerID CHAR(5) NOT NULL,
    PurchaseDate DATE NOT NULL,
    Status VARCHAR(50) NOT NULL,
    CONSTRAINT PurchaseTransaction_StaffID_FK FOREIGN KEY (StaffID) REFERENCES Staff(StaffID),
    CONSTRAINT PurchaseTransaction_DesignerID_FK FOREIGN KEY (DesignerID) REFERENCES Designer(DesignerID),
    CONSTRAINT PurchaseTransaction_PurchaseDate_CK CHECK(EXTRACT(MONTH FROM PurchaseDate) IN (1, 3, 4, 6, 8, 11)),
    CONSTRAINT PurchaseTransaction_PurchaseID_CK CHECK(REGEXP_LIKE(PurchaseTransactionID, '^PU[0-9]{3}$'))
);

CREATE TABLE PurchaseTransactionDetail (
    ItemID CHAR(5) NOT NULL,
    PurchaseTransactionID CHAR(5) NOT NULL,
    Quantity INT NOT NULL, 
    CONSTRAINT PurchaseTransactionDetail_ItemID_PurchaseTransactionID_PK PRIMARY KEY (PurchaseTransactionID, ItemID),
    CONSTRAINT PurchaseTransactionDetail_ItemID_FK FOREIGN KEY (ItemID) REFERENCES FashionItems(ItemID),
    CONSTRAINT PurchaseTransactionDetail_PurchaseTransactionID_FK FOREIGN KEY (PurchaseTransactionID) REFERENCES PurchaseTransaction(PurchaseTransactionID)
);

CREATE TABLE SalesTransaction (
    salesTransactionID CHAR(5) PRIMARY KEY,
    CustomerID CHAR(5) NOT NULL,
    StaffID CHAR(5) NOT NULL,
    SalesDate DATE NOT NULL,
    Status VARCHAR(50) NOT NULL,
    CONSTRAINT SalesTransaction_salesTransactionID_CK CHECK(REGEXP_LIKE(salesTransactionID, '^SL[0-9]{3}$')),
    CONSTRAINT SalesTransaction_DateMonth_CK CHECK(EXTRACT(MONTH FROM SalesDate) IN (1, 3, 4, 6, 8, 11)),
    CONSTRAINT SalesTransaction_CustomerID_FK FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID),
    CONSTRAINT SalesTransaction_StaffID_FK FOREIGN KEY (StaffID) REFERENCES Staff(StaffID)
);

CREATE TABLE SalesTransactionDetail (
    SalesTransactionID CHAR(5) NOT NULL,
    ItemID CHAR(5) NOT NULL,
    Quantity INT NOT NULL, 
    CONSTRAINT SalesTransactionDetail_SalesTransactionID_ItemID_PK PRIMARY KEY (SalesTransactionID, ItemID),
    CONSTRAINT SalesTransactionDetail_SalesTransactionID_FK FOREIGN KEY (SalesTransactionID) REFERENCES SalesTransaction(SalesTransactionID),
    CONSTRAINT SalesTransactionDetail_ItemID_FK FOREIGN KEY (ItemID) REFERENCES FashionItems(ItemID)
);

INSERT ALL 
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA001', 'Evening Wear', 'Elegant attire for evening occasions')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA002', 'Streetwear', 'Casual street fashion')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA003', 'Bridal Wear', 'Wedding and ceremonial gowns')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA004', 'Workwear', 'Everyday work fashion')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA005', 'Vintage', 'Old-school fashion styles')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA006', 'Sportswear', 'Athletic and active wear')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA007', 'Punk', 'Edgy and alternative fashion')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA008', 'Maternity Wear', 'Fashion for pregnant women')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA009', 'Loungewear', 'Comfortable home wear')
    INTO FashionCategories(CategoryID, CategoryName, CategoryDescription) VALUES ('CA010', 'Winter Wear', 'Clothing for cold weather')
SELECT * FROM DUAL

INSERT ALL
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA001', 'Elegant Gown', 'Red evening dress with silk fabric', 1000, 1800, 10, 'CA001')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA002', 'Denim Jacket', 'Casual streetwear denim jacket', 800, 1200, 15, 'CA002')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA003', 'Wedding Dress', 'Full lace bridal dress', 2000, 3500, 10, 'CA003')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA004', 'T-Shirt', 'Basic white t-shirt', 1500, 2500, 30, 'CA004')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA005', 'Retro Dress', '1970s style polka dot dress', 800, 1500, 10, 'CA005')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA006', 'Running Shorts', 'Lightweight shorts for jogging', 2000, 4000, 20, 'CA006')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA007', 'Leather Jacket', 'Punk-style black leather jacket', 1000, 2000, 12, 'CA007')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA008', 'Maternity Dress', 'Stretchable cotton dress for mothers', 600, 900, 12, 'CA008')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA009', 'Hoodie', 'Soft fleece hoodie for lounging', 750, 1000, 25, 'CA009')
  INTO FashionItems(ItemID, ItemName, ItemDescription, ItemPurchasePrice, ItemSalesPrice, ItemStock, CategoryID) VALUES ('FA010', 'Wool Coat', 'Long winter coat with lining', 1200, 2200, 10, 'CA010')
SELECT * FROM DUAL;

INSERT ALL
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU001', 'Duran Moore', 'Male', 'Jl. Merdeka 1', 'duran@gmail.com', '081234567890')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU002', 'Alice Tan', 'Female', 'Jl. Mawar 3', 'alice.tan@gmail.com', '081223344556')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU003', 'Michael Surya', 'Male', 'Jl. Cemara 7', 'mike.s@gmail.com', '083598765432')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU004', 'Olivia Chen', 'Female', 'Jl. Bambu 9', 'oliviachen@gmail.com', '081655667788')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU005', 'David Lee', 'Male', 'Jl. Sukarno 10', 'david.l@gmail.com', '081644466699')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU006', 'Adeline', 'Female', 'Jl. Pelangi 6', 'ninaw@gmail.com', '081288899900')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU007', 'George Howard', 'Male', 'Jl. Bintaro 2', 'georgeh@gmail.com', '081211122233')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU008', 'Fiona Angel', 'Female', 'Jl. Anggrek 5', 'fiona.a@gmail.com', '081277788899')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU009', 'Brian Lee', 'Male', 'Jl. Seruni 4', 'briank@gmail.com', '081200112233')
    INTO Customer(CustomerID, CustomerName, CustomerGender, CustomerAddress, CustomerEmail, CustomerPhone ) VALUES ('CU010', 'Grace Lee', 'Female', 'Jl. Kenanga 11', 'gracelee@gmail.com', '081233344455')
SELECT * FROM DUAL;

INSERT ALL
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE001', 'Amanda Liu', 'Female', 'Tambora 3', 'amanda@modesign.com', '081233344455')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE002', 'Steven Wong', 'Male', 'Duri Kepa 2', 'steven@modesign.com', '081288899911')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE003', 'Clara Tan', 'Female', 'Daan Mogot Raya', 'clara@modesign.com', '081233311122')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE004', 'Raymond Chen', 'Male', 'Jalan Bambu 4', 'raymond@modesign.com', '081255566677')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE005', 'Cynthia Halim', 'Female', 'Teluk Gong', 'cynthia@modesign.com', '081266677788')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE006', 'Jami', 'Male', 'Lumbini 78', 'james@modesign.com', '081211122211')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE007', 'Vero', 'Female', 'Dankee 89', 'veronica@modesign.com', '081233366699')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE008', 'Daniel Rey', 'Male', 'Donga Jaya 8', 'daniel@modesign.com', '081244455566')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE009', 'Angela Wu', 'Female', 'Duke Univ 65', 'angela@modesign.com', '081233388800')
    INTO Designer(DesignerID, DesignerName, DesignerGender, DesignerAddress, DesignerEmail, DesignerPhone) VALUES ('DE010', 'Felix Tan', 'Male', 'Hujan 10', 'felix@modesign.com', '081266644455')
SELECT * FROM DUAL;

INSERT ALL
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM001', 'Samuel Mick', 'Male', 'Jl. Satria 1', 'samuel@mostaff.com', '081233344411')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM002', 'Sandra Lee', 'Female', 'Jl. Patimura 2', 'sandra@mostaff.com', '081244455566')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM003', 'Richard Wang', 'Male', 'Jl. Sudirman 3', 'richard@mostaff.com', '081255566677')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM004', 'Claudia Kim', 'Female', 'Jl. Diponegoro 4', 'claudia@mostaff.com', '081266677788')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM005', 'Kevin Yeb', 'Male', 'Jl. Melati 5', 'kevin@mostaff.com', '081277788899')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM006', 'Bella Tan', 'Female', 'Jl. Mawar 6', 'bella@mostaff.com', '081288899900')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM007', 'Leonard Chris', 'Male', 'Jl. Sakura 7', 'leonard@mostaff.com', '081211112222')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM008', 'Natalie Port', 'Female', 'Jl. Semangka 8', 'natalie@mostaff.com', '081222233344')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM009', 'Steven Chen', 'Male', 'Jl. Gandaria 9', 'stevenc@mostaff.com', '081233344455')
    INTO Staff(StaffID, StaffName, StaffGender, StaffAddress, StaffEmail, StaffPhone) VALUES ('SM010', 'Monica Surya', 'Female', 'Jl. Nangka 10', 'monica@mostaff.com', '081244455566')
SELECT * FROM DUAL;

INSERT ALL
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU001', 'SM001', 'DE001', TO_DATE('10-01-2024','DD-MM-YYYY'), 'Completed')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU002', 'SM002', 'DE002', TO_DATE('05-03-2024', 'DD-MM-YYYY'), 'Completed')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU003', 'SM003', 'DE003', TO_DATE('20-04-2024', 'DD-MM-YYYY'), 'Pending')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU004', 'SM004', 'DE004', TO_DATE('18-06-2024', 'DD-MM-YYYY'), 'Completed')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU005', 'SM005', 'DE005', TO_DATE('12-08-2024', 'DD-MM-YYYY'), 'Completed')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU006', 'SM006', 'DE006', TO_DATE('02-11-2024', 'DD-MM-YYYY'), 'Pending')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU007', 'SM007', 'DE007', TO_DATE('15-03-2024', 'DD-MM-YYYY'), 'Completed')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU008', 'SM008', 'DE008', TO_DATE('22-04-2024', 'DD-MM-YYYY'), 'Completed')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU009', 'SM009', 'DE009', TO_DATE('25-08-2024', 'DD-MM-YYYY'), 'Completed')
  INTO PurchaseTransaction(PurchaseTransactionID, StaffID, DesignerID, PurchaseDate, Status) VALUES ('PU010', 'SM010', 'DE010', TO_DATE('30-01-2024', 'DD-MM-YYYY'), 'Pending')
SELECT * FROM DUAL;

INSERT ALL
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA001', 'PU001', 5)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA002', 'PU002', 3)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA003', 'PU003', 2)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA004', 'PU004', 4)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA005', 'PU005', 1)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA006', 'PU006', 6)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA007', 'PU007', 2)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA008', 'PU008', 3)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA009', 'PU009', 5)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA010', 'PU010', 2)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA002', 'PU001', 1)
  INTO PurchaseTransactionDetail(ItemID, PurchaseTransactionID, Quantity) VALUES ('FA003', 'PU002', 2)
SELECT * FROM DUAL;

INSERT ALL
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL001', 'CU001', 'SM001', TO_DATE('03-04-2024', 'DD-MM-YYYY'), 'Completed')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL002', 'CU002', 'SM002', TO_DATE('07-04-2024', 'DD-MM-YYYY'), 'Completed')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL003', 'CU003', 'SM003', TO_DATE('12-06-2024', 'DD-MM-YYYY'), 'Pending')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL004', 'CU004', 'SM004', TO_DATE('05-08-2024', 'DD-MM-YYYY'), 'Completed')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL005', 'CU005', 'SM005', TO_DATE('18-01-2024', 'DD-MM-YYYY'), 'Completed')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL006', 'CU006', 'SM006', TO_DATE('02-11-2024', 'DD-MM-YYYY'), 'Completed')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL007', 'CU007', 'SM007', TO_DATE('15-03-2024', 'DD-MM-YYYY'), 'Completed')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL008', 'CU008', 'SM008', TO_DATE('22-04-2024', 'DD-MM-YYYY'), 'Completed')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL009', 'CU009', 'SM009', TO_DATE('25-08-2024', 'DD-MM-YYYY'), 'Pending')
  INTO SalesTransaction(salesTransactionID, CustomerID, StaffID, SalesDate, Status) VALUES ('SL010', 'CU010', 'SM010', TO_DATE('30-01-2024', 'DD-MM-YYYY'), 'Completed')
SELECT * FROM DUAL;

INSERT ALL
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL001', 'FA001', 2)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL002', 'FA002', 1)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL003', 'FA003', 1)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL004', 'FA004', 3)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL005', 'FA005', 2)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL006', 'FA006', 1)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL007', 'FA007', 2)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL008', 'FA008', 1)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL009', 'FA009', 3)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL010', 'FA010', 2)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL001', 'FA002', 1)
  INTO SalesTransactionDetail(SalesTransactionID, ItemID, Quantity) VALUES ('SL002', 'FA003', 2)
SELECT * FROM DUAL;

-- SOAL 3
SELECT CU.CustomerName, SA.StaffName, COUNT(ST.SalesTransactionID) AS TotalTransaction 
FROM Customer CU
JOIN SalesTransaction ST ON CU.CustomerID = ST.CustomerID
JOIN Staff SA ON ST.StaffID = SA.StaffID
WHERE EXTRACT(MONTH FROM ST.SalesDate) = 4 AND SA.StaffName LIKE 'S%'
GROUP BY CU.CustomerName, SA.StaffName

-- SOAL 4
SELECT FC.CategoryName, CONCAT('$',SUM(FI.ItemPurchasePrice * PTD.Quantity)) AS TotalPrice
FROM FashionCategories FC
JOIN FashionItems FI ON FC.CategoryID = FI.CategoryID
JOIN PurchaseTransactionDetail PTD ON FI.ItemID = PTD.ItemID
WHERE FC.CategoryName LIKE '%e%'
GROUP BY FC.CategoryName
HAVING SUM(FI.ItemPurchasePrice * PTD.Quantity) > 2800

-- SOAL 5
SELECT TO_CHAR(PT.PurchaseDate, 'dd Mon yyyy') AS "Date", DE.DesignerName, COUNT(FI.ItemID) AS TotalFashionItems, SUM(PTD.Quantity) AS TotalQuantity
FROM PurchaseTransaction PT
JOIN Designer DE ON PT.DesignerID = DE.DesignerID
JOIN PurchaseTransactionDetail PTD ON PT.PurchaseTransactionID = PTD.PurchaseTransactionID
JOIN FashionItems FI ON PTD.ItemID = FI.ItemID
WHERE LENGTH(DE.DesignerName) > 4 AND EXTRACT(MONTH FROM PT.PurchaseDate) = 8
GROUP BY DE.DesignerName, PT.PurchaseDate

-- SOAL 6
SELECT SA.StaffName, FI.ItemName, COUNT(FI.ItemID) AS TotalFashionItems, CONCAT('$', (FI.ItemSalesPrice * STD.Quantity)) AS TotalPrice
FROM SalesTransaction ST
JOIN Staff SA ON ST.StaffID = SA.StaffID
JOIN SalesTransactionDetail STD ON ST.SalesTransactionID = STD.SalesTransactionID
JOIN FashionItems FI ON STD.ItemID = FI.ItemID
WHERE SA.StaffGender = 'Male' AND ST.SalesTransactionID IN (
      SELECT ST2.SalesTransactionID
      FROM SalesTransaction ST2
      JOIN SalesTransactionDetail STD2 ON ST2.SalesTransactionID = STD2.SalesTransactionID
      JOIN Staff SA2 ON ST2.StaffID = SA2.StaffID
      WHERE SA2.StaffGender = 'Male'
      GROUP BY ST2.SalesTransactionID
      HAVING COUNT(STD2.ItemID) < 2
  )
GROUP BY SA.StaffName, FI.ItemName, FI.ItemSalesPrice, STD.Quantity
ORDER BY (FI.ItemSalesPrice * STD.Quantity) DESC;

-- SOAL 7
SELECT DISTINCT DE.DesignerName, DE.DesignerAddress, DE.DesignerEmail, FI.ItemName
FROM Designer DE
JOIN PurchaseTransaction PT ON DE.DesignerID = PT.DesignerID
JOIN PurchaseTransactionDetail PTD ON PT.PurchaseTransactionID = PTD.PurchaseTransactionID
JOIN FashionItems FI ON PTD.ItemID = FI.ItemID
WHERE FI.ItemPurchasePrice = (
    SELECT MIN(ItemPurchasePrice)
    FROM FashionItems
) AND DE.DesignerAddress LIKE 'D%' 
ORDER BY DE.DesignerName ASC

-- SOAL 8
SELECT DISTINCT CU.CustomerName, CU.CustomerEmail, CU.CustomerPhone, FI.ItemName, CONCAT('$', FI.ItemSalesPrice) AS ItemPrice 
FROM Customer CU
JOIN SalesTransaction ST ON CU.CustomerID = ST.CustomerID
JOIN SalesTransactionDetail STD ON ST.SalesTransactionID = STD.SalesTransactionID
JOIN FashionItems FI ON STD.ItemID = FI.ItemID
WHERE FI.ItemSalesPrice = (
    SELECT MIN(ItemSalesPrice)
    FROM FashionItems
) AND CU.CustomerName LIKE '%o%'
ORDER BY CU.CustomerName ASC

-- SOAL 9
SELECT SA.StaffName, TO_CHAR(ST.SalesDate, 'dd Mon yyyy') AS "Date", FI.ItemName, CONCAT('$', SUM(FI.ItemSalesPrice * STD.Quantity)) AS TotalPrice
FROM Staff SA
JOIN SalesTransaction ST ON SA.StaffID = ST.StaffID
JOIN SalesTransactionDetail STD ON ST.SalesTransactionID = STD.SalesTransactionID
JOIN FashionItems FI ON STD.ItemID = FI.ItemID
WHERE LENGTH(SA.StaffName) > (
    SELECT AVG(LENGTH(StaffName))
    FROM Staff
) AND MOD(TO_NUMBER(TO_CHAR(ST.SalesDate, 'dd')), 2) = 1
GROUP BY SA.StaffName, TO_CHAR(ST.SalesDate, 'dd Mon yyyy'), FI.ItemName

-- SOAL 10
SELECT UPPER(DE.DesignerName) AS DesignerName, PT.PurchaseTransactionID, CONCAT('$', (FI.ItemPurchasePrice * PTD.Quantity)) AS TotalPrice
FROM Designer DE
JOIN PurchaseTransaction PT ON DE.DesignerID = PT.DesignerID
JOIN PurchaseTransactionDetail PTD ON PT.PurchaseTransactionID = PTD.PurchaseTransactionID
JOIN FashionItems FI ON PTD.ItemID = FI.ItemID
WHERE (FI.ItemPurchasePrice * PTD.Quantity) < (
    SELECT AVG(ItemPurchaseprice)
    FROM FashionItems
) AND DE.DesignerName LIKE '%a%'
ORDER BY DE.DesignerName DESC
