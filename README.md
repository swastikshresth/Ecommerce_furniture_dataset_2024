# Ecommerce_furniture_dataset_2024

**Import Data into SQL Database**
1. Prepare a csv file
2. create table in SQL
3. Import csv into sql

**ETL(Extract Transform  and Load)**

**1. Extract**
   a. Extract the data from the source system as a csv file and import into SQL database
   b. Ensure the data is correctly loaded into a SQL table with appropriate initial schema.
**2. Transform**
   In the Transform stage, we primarily focus on cleaning the data and converting it into meaningful, insightful information for analysis.
   # Data Cleaning
   In Data cleaning, I'm mentioning all queries which I have used in this project for cleaning the data 
   1. ✅ **Step 1 — Remove `$` sign from both columns**
     sql
        UPDATE e_commerce_furniture_dataset
        SET Original_Price = REPLACE(Original_Price, '$', '');

        UPDATE e_commerce_furniture_dataset
        SET Price = REPLACE(Price, '$', '');
      
   3. **step 2 - Remove ',' from String and REPLACE with '' **
      update e_commerce_furniture_dataset set original_price =replace(original_price,',','');

   4. ✅ **Step 3 — Remove extra spaces (in case of any accidental whitespaces)**
      sql
      UPDATE e_commerce_furniture_dataset
      SET Original_Price = TRIM(Original_Price);

      UPDATE e_commerce_furniture_dataset
      SET Price = TRIM(Price);
   
  5. ✅ **Step 4 — Replace empty strings with NULL**
     sql
     UPDATE e_commerce_furniture_dataset
     SET Original_Price = NULL
     WHERE Original_Price = '' OR Original_Price IS NULL;

     UPDATE e_commerce_furniture_dataset
     SET Price = NULL
     WHERE Price = '' OR Price IS NULL;
     
 6.   **Step 5 — Replace NULLs with 0 (only if you want to handle nulls this way)**
      sql
      UPDATE e_commerce_furniture_dataset
      SET Original_Price = 0
      WHERE Original_Price IS NULL;

      UPDATE e_commerce_furniture_dataset
      SET Price = 0
      WHERE Price IS NULL;
 7.   ✅ **Step 6 — Convert datatype to DECIMAL**
      sql
      ALTER TABLE e_commerce_furniture_dataset
      MODIFY COLUMN Original_Price DECIMAL(10,2);

      ALTER TABLE e_commerce_furniture_dataset
      MODIFY COLUMN Price DECIMAL(10,2);

 8. **Step 7 - Add one more column i.e ID which is primary so that I can delete the duplicate rows**
             Alter table e_commerce_furniture_dataset add column ID int AUTO_INCREMENT primary key;

 9. Step -8 - Check duplicates values
             select Product_Title,original_price,price,sold,Tag_Text,count(*) as Duplicates 
			    from e_commerce_furniture_dataset group by Product_Title,original_price,price,sold,Tag_Text 
			    having COUNT(*)>1;

10.  Delete duplicate rows
            Delete from e_commerce_furniture_dataset 
			   where id not in (select min(id) from e_commerce_furniture_dataset group by Product_Title,Original_Price,price,sold,Tag_Text)


11. **Check null values**
         select Product_Title,Original_Price,price,sold,Tag_Text,count(*) as Null_Count from e_commerce_furniture_dataset
			 where Product_Title is null or price is null or Original_Price is null or sold is null or Tag_Text is null 
			 group by Product_Title,Original_Price,price,sold,Tag_Text;
			 OR 
			  SELECT  SUM(Product_Title IS NULL) AS Product_Title_Nulls,
                   SUM(Original_Price IS NULL) AS Original_Price_Nulls,
                   SUM(price IS NULL) AS Price_Nulls,
                   SUM(sold IS NULL) AS Sold_Nulls,
                   SUM(Tag_Text IS NULL) AS Tag_Text_Nulls
                   FROM e_commerce_furniture_dataset;
    
13. Step 11 - **Remove White spaces **
               update e_commerce_furniture_dataset 
               set Product_Title=trim(Product_Title),
               Original_Price=trim(Original_Price),
               price=trim(price),
               sold=trim(sold),
               Tag_Text=trim(Tag_Text);











      
