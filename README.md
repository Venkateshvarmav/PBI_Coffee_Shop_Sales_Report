# PBI_Coffee_Shop_Sales_Report


# Coffee Shop Sales

Dashboard Link - https://app.powerbi.com/groups/me/reports/2fb98590-42f4-4b51-be44-6c55404ed16f/b6e7faa788c2079b6b27?experience=power-bi&clientSideAuth=0

## Problem Statement

This dashboard helps the coffee shop understand their customers better. It helps the coffee know their customer's preference and behaviour. Through different parameters, they get to know their best and worst product in each store location and their busiest day and time to assign more employees & thus they can improve their services. It also lets them know the sales trend and revenue generated each month/day and comparing them with previous month/day.


### Steps followed 

- Step 1 : Load data into Power BI Desktop, dataset is a excel file.
- Step 2 : Open power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options.
- Step 3 : Also since by default, profile will be opened only for 1000 rows so you need to select "column profiling based on entire dataset" to check the empty and error percentage.
- Step 4 : It was observed that in none of the columns errors & empty values were present
- Step 5 : Since the data contains various products and categories, thus in order to represent sales, a new visual was added using the bar graph in the visualizations pane in report view. 
- Step 6 : Visual filters (Slicers) were added for the month field.
- Step 7 : Metrics table was used as a heat map to show the most and least sales for that particular calendar month
- Step 8 : Bar chart were used to show sales by store location,product category, top 10 product type, sales by hour of the day and sales trend over the preiod.
- Step 9 : Tool Tips are used to show the sales, quanitity sold and total orders using cards and donut charts
- Step 10 : Line graph is used for Spark lines show the trend(sales, qty sold, orders) for the month
- Step 11 : Donut chart is used for showing the sales on weekday against weekend


- Step 12 : For creating new columns following DAX expressions were written;

to extract hour from the transaction time-
```DAX
HOUR = HOUR(Transactions[transaction_time])
```

to identify the sales amount
```DAX
Sales = Transactions[unit_price]*Transactions[transaction_qty]
```
        
- Step 13 : New measure was created to find total the current month orders and previous month orders.`

Following DAX expression was written for the same,

```DAX
Current Month Orders = 
var selected_month = SELECTEDVALUE('Date Table'[Month])
return 
TOTALMTD(CALCULATE([Total Orders],'Date Table'[Month]=selected_month),'Date Table'[Date])
```

```DAX
Previous Month Orders = 
CALCULATE([Current Month Orders],DATEADD('Date Table'[Date],-1,MONTH))
```

**Similar measure were written for Total Sales and Total Quantity replacing the associated column names.**

A New card visual was used to represent the above measures


- Step 14 : New measure was created to find Daily avg sales using
```DAX
Daily avg sales = AVERAGEX(ALLSELECTED(Transactions[transaction_date]),[Total Sales])
```
 
 - Step 15 : Following DAX expression was written for label for product category, product type and store location
 
```DAX
Label for product category = SELECTEDVALUE(Transactions[product_category]) & " | " & FORMAT([Total Sales]/1000,"$0.00K")
```

**Similar DAX were written for product type and store location by replacing the column names.**
  
 - Step 17 : New measure was created to calculate for Month on month difference with respect to growth and difference of orders
 
```DAX
MoM Growth & Difference of Orders = 
VAR monthdiff = [Current Month Orders]-[Previous Month Orders]
var MoM = monthdiff/[Previous Month Orders]
var _sign = IF(monthdiff>0,"+","")
var sign_trend = IF(monthdiff>0,"▲","▼")
return 
IF(SELECTEDVALUE('Date Table'[Month Year]) = "Jan 2023","No value",
sign_trend & " " & _sign & FORMAT(MoM,"#0.0%" & " " & "|" &" " & _sign & FORMAT(monthdiff/1000,"0.0K")) & " " & "vs LM")
```

 **Similar measures written for Total Sales and Total Quantity by replacing the column names**
 
 
# Snapshot of Dashboard (Power BI Service)

![image](https://github.com/user-attachments/assets/091854af-c40e-4fca-8983-8e68bba60d71)

 
 # Report Snapshot (Power BI DESKTOP)

  ![image](https://github.com/user-attachments/assets/35c15ea9-f825-4554-8577-1fce908b8e9c)


# Insights

A single page report was created on Power BI Desktop with 2 tooltips & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;

### [1] Total Sales for the month of May 

   Total Sales for May - $157K
   
   Total Sales went up by 31.8% in the month of May when compared to April - $37.8K

### [2] Total Orders for the month of May 

   Total Orders for May - 33527
   
   Total Orders went up by 32.3% in the month of May when compared to April - 8.2K
   
### [3] Total Sales for the month of May 

   Total Quantity for May - 48233
   
   Total Quantity went up by 32.3% in the month of May when compared to April - 11.8K

           
###  [4] Sales Compared to Weekends by Weekdays

Sales in Weekdays contributes to 74.41% with weekends contributing 25.59%
  
  
### [5] Top performing Store Locations in May
  
Hell's Kitchen - $56.96K
Astoria - $55.08K
Lower Manhattan - $54.45K

### [6] Some other insights

**Top 3 performing product sales by category**

Coffee -$64.9K
Tea - $46.24K
Bakery - 19.25K

**Bottom 3 performing product sales by category**

Loose tea - $2.77K
Flavours - $2.0K
Packaged Choc0late - $0.99K

**Top 3 performing product Type by category**

Barista Espresso - $21.66K
Brewed Chai Tea - $18.19
Gourmet brewed Coffee - $17.14K

**Bottom 3 performing product sales by category**

Organic brewed Coffee - $8.78K
Scone - $8.55K
Drip Coffee - $7.77K

### Staff Assignment

From the heat map it can be noticed that 
* Friday,Thursday and Wednesday between 7 AM to 11 AM is the busiest at **Hell's Kitchen**
* Friday,Thursday, Saturday and Monday between 7 AM to 10 AM is the busiest at **Astoria**
* Friday,Thursday, Tuesday between 7 AM to 10 AM is the busiest at **Lower Manhattan**

 With this data the store manager can allocate more employee between that time for faster service
