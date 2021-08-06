# cat_challenge

The task was to summarize by product id weights of delivered products that belong to filtered product categories and selected date range.

In the beginning, 6 data sets were provided and loaded into the PowerBI app: **customers_dataset**, **order_items_dataset**, **orders_dataset**, **products_dataset**, **sellers_dataset**, **targets**. 
In addition, **dimCalendar** table we generated in **PowerQuery**. As the requirement was to calculate delivered weights by a date range, column '**order_delivered_customer_date**' was used for joining with **dimCalendar** table (as this column was in the timestamp format, the date part was extracted from it into the '**delivered_final_date**' column). After the load, **order_items_dataset** was joined to the **orders_dataset** into a single fact table. In addition, column '**product_weight_g**' from **products_dataset** was joined to the same table. Since the '**product_categories**' column in **products_dataset** can contain more than one category separated by a comma, it was parsed in a separate table (**products_categories**) where each category was split into a separate row. Also, a separate table was created for storing **DAX** measures (**a_measures**).

![image](https://user-images.githubusercontent.com/56403895/128514492-ba59e2e2-c175-4e3c-9534-27f622e72592.png)


**Intro** 


The **Intro** page contains a general overview of the numbers: **Weights Delivered**, **Quantity Delivered**,** Number of Orders canceled**, **Weights canceled**, **Top 5 categories by delivered quantity**, **Top 5 categories by delivered weights**, and **Top 5 categories by Income**. The idea behind this page was to provide a general overview of the data before answering the main requirement from the assignment. It also contains the button that leads to this page. 

![Uploading image.png…]()


**Weights analysis**

This page contains a table that shows **product number**, **product id**, **single unit weight**, **quantity delivered** and **weight delivered**. 

Calculation for Quantity delivered: 
no_of_items = 
CALCULATE(COUNT(orders_dataset[product_id]),
FILTER(orders_dataset, orders_dataset[order_status]="delivered"))

Calculation for Weights delivered:
total_weight = 
CALCULATE(SUM(orders_dataset[product_weight_g]),
FILTER(orders_dataset, orders_dataset[order_status]="delivered"))

Two filters were added to the page: **product_category** and **date range**, which allow users to further filter the data. 


**Data Quality Check and Validation**

After the load, the data were inspected and checked for any anomalies or missing values in key columns, e.g. **weights**, **order_devilery_date**, **product_category**, etc. The following issues were found: In total, **610** different products don't have a **category**; There were several orders that were delivered but don't have '**order_delivered_customer_date**' filled in; There were a couple of orders that didn't have any order lines (order_items_dataset entries) but were in a status invoiced or shipped. Other than that no major data issues were discovered (no null values for weights, product_id, etc). Data check results can be seen on the **'Data Quality Check and Validation'** page of the PowerBI app.



