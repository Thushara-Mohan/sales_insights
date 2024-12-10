
# sales_insights

A report of sales and revenue of a company

Database i used: `db_dump.sql` from an youtube video

### Data Analysis Using SQL

1. Show all customer records

    `SELECT * FROM customers;`

2. Show total number of customers

    `SELECT count(*) FROM customers;`

3. Show transactions for Chennai market (market code for chennai is Mark001

    `SELECT * FROM transactions where market_code='Mark001';`

4. Show distrinct product codes that were sold in chennai

    `SELECT distinct product_code FROM transactions where market_code='Mark001';`

5. Show transactions where currency is US dollars

    `SELECT * from transactions where currency="USD"`

6. Show transactions in 2020 join by date table

    `SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020;`

7. Show total revenue in year 2020,

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.currency="INR\r" or transactions.currency="USD\r";`
	
8. Show total revenue in year 2020, January Month,

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and and date.month_name="January" and (transactions.currency="INR\r" or transactions.currency="USD\r");`

9. Show total revenue in year 2020 in Chennai

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.market_code="Mark001";`


Data Analysis Using Power BI
============================

step 1: loaded the data into powerBi from mysql Database
step 2: filtered unwanted values from some columns

    - In the sales.transactions table, there was sales_amount
     column whose values are 0 or negative values.
    - since i needed only data related to India, i filtered out
     other columns which are not Indian states.
step 3: Adressed some problems related to data

#### Problem1:
The date table was having two columns with similar values, so i transformed "cy_date" column to mmm yy format for easy understanding and usage in visualization.

#### Problem2: 
The data had a few values of currency in "USD" and the rest were in "INR" also the two of the above mentioned currency values had 2 different values one simply the name of the currency and the other one with a newline character in it.

#### Solution:
- here we removed the duplicate values of currency column
- created a new column called normalized curency column with all the values converted into "INR" ("USD" multiplied with the corresponding conversion rate to "INR")

    Formula to create normalized_currency column in Power BI

    `= Table.AddColumn(#"Filtered Rows", "normalized_amount", each if [currency] = "USD" or [currency] ="USD#(cr)" then [sales_amount]*84.88 else [sales_amount], type any)`

Now we have a detailed data without errors

step 4: Created dashboard with useful data patterns and content needed to be analyzed to discover the sales of the company

snap shot of my dashboard

![snap_dashboard](https://github.com/user-attachments/assets/95a8e1cf-3147-435d-9c1c-b03e1f7bd7fa)

revenue of year 2020

![revenue_2020](https://github.com/user-attachments/assets/68114bc2-1647-4d88-b4f8-26f8bb262ae9)

