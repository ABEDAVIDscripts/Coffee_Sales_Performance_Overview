# Power BI Solution Section Only

<BR>

### Canvas Configuration
#### STEP 1: Import and transform data
- In Transform Data, check for data quality under View Tab: using column quality and distribution
- Close and apply
- Set the transaction_time datatype to hh:nn:ss


<BR>

#### STEP 2: Canvas Configuration
- Add canvas background
- In canvas setting, set Type to custom: (H: 850, W: 1400) 
- Vertical alignment: middle
- Canvas background - image fit : select fit

<br>

#### STEP 3: Create A Date Table
```DAX
DateTable = ADDCOLUMNS (
	CALENDAR ( MIN('YourDataTable'[Date]), MAX('YourDataTable'[Date]) ),
	"Year", YEAR([Date]),
	"Month", FORMAT([Date], "MMMM"),
	"MonthNum", MONTH([Date]),
	"Quarter", "Q" & FORMAT([Date], "Q"),
	"Weekday", FORMAT([Date], "dddd"),
	"WeekdayNum", WEEKDAY([Date], 2) )
```


<BR>

#### STEP 4: Data Modeling
- Add a relationship between Date Table and Transactions table
- Under Model View, connect the 2 tables
- Drag the date in the Date Table on the transaction_date in Transactions Table
- The relationship is Many to One.


<Br>
<Br>



## Business Questions: KPI Section

### 1. Total Sales Analysis with month-on-month increase or decrease in sales
- create total sales measure
```DAX
Total Sales = SUMX(Transactions, Transactions[unit_price] * Transactions[transaction_qty])
```

- Create "Previous Month Sales" Measure
```DAX
Previous Month Sales = CALCULATE([Total Sales], PREVIOUSMONTH(DateTable[Date]))
```

- Create "MoM Sales Difference" Measure
```DAX
MoM Sales Difference = [Total Sales] - [Previous Month Sales]
```

- Create "MoM Sales % Change" Measure
```DAX 
MoM Sales % Change = 
	IF( NOT(ISBLANK([Previous Month Sales])), 
	DIVIDE([MoM Sales Difference], [Previous Month Sales], 0), 
	BLANK() )	
```

- Add visualization card, Add Total Sales on it. Then format the card

- Create a new column for Month-Year:
```DAX
Month-Year = FORMAT(DateTable[Date], "MMM YYYY")
```

- Add slicer, Add Month-Year column to the slicer. Format and Sort the slicer

- Total Sales KPI to display the MoM diff & percentage 
```DAX
KPI Total Sales = 
	VAR MoM_Diff = [MoM Sales Difference]
	VAR Mom_Percent = [MoM Sales % Change]
	VAR CurrentMonth = SELECTEDVALUE('DateTable'[MonthNumb])

	RETURN
	IF(
	CurrentMonth = 1,
	BLANK(), 
	FORMAT(MoM_Diff/1000, "0.0K") & " | " & FORMAT(mom_percent, "#0.0%") & " " & "MoM diff" & " " & "vs LM" )
```

- Create a card, drag on KPI Total Sales
- Format the card to merge it with Total Sales Card

<br>

### 2. Total Orders Analysis

- Create Total orders measure
```DAX
	Total Orders = DISTINCTCOUNT(Transactions[transaction_id])
```

- Create the Previous Month Orders Measure
```DAX
	 Orders Previous Month = CALCULATE([Total Orders], DATEADD('DateTable'[Date], -1, MONTH) )
```

- Calculate the Month-over-Month (MoM) Difference
```DAX
	MoM Orders Difference = [Total Orders] - [Orders Previous Month]
```

- Calculate the Month-over-Month Percentage Change
```DAX
	MoM Order % Change = 
	IF( NOT(ISBLANK([Orders Previous Month])), DIVIDE([MoM Orders Difference], [Orders Previous Month], 0), BLANK())
```

- Orders MoM KPI
```DAX
	KPI Total Orders = 
		VAR MoM_Diff = [MoM Orders Difference]
		VAR Mom_Percent = [MoM Order % Change]
		VAR CurrentMonth = SELECTEDVALUE('DateTable'[MonthNum])
		RETURN
		IF( CurrentMonth = 1, BLANK(), 
		MoM_Diff & " | " & FORMAT(mom_percent, "#0.0%") & " " & "MoM diff" & " " & "vs LM" )
```

- Add card, Add Total Orders on it. Then format the card
- Add another card, drag on Orders MoM KPI
- Format the card to merge it with Total Orders Card

<br>


### 3. Total Quantity Sold Analysis

- Create the Total Quantity Sold Measure
```DAX
	Total Quantity Sold = SUM(Transactions[transaction_qty])
```


- Create the Previous Month Quantity Sold Measure
```DAX
	Previous Month Quantity Sold = CALCULATE([Total Quantity Sold], DATEADD('DateTable'[Date], -1, MONTH))
```

- Calculate the Month-over-Month (MoM) Difference
```DAX
	MoM Quantity Difference = [Total Quantity Sold] - [Previous Month Quantity Sold]
```

- Calculate the Month-over-Month Percentage Change
```DAX
	MoM Quantity % Change = 
	IF(
		NOT(ISBLANK([Previous Month Quantity Sold])),
		DIVIDE([MoM Quantity Difference], [Previous Month Quantity Sold], 0), BLANK() )
```

- Total Quantity mom KPI 
```DAX
	KPI Total Quantity = 
		VAR MoM_Diff = [MoM Quantity Difference]
		VAR Mom_Percent = [MoM Quantity % Change]
		VAR CurrentMonth = SELECTEDVALUE('DateTable'[MonthNum])
		RETURN
		IF( CurrentMonth = 1, BLANK(), 
		MoM_Diff & " | " & FORMAT(mom_percent, "#0.0%") & " " & "MoM Diff" & " " & "vs LM" )
```

- Add card, Add Total Quantity on it. Then format the card
- Add another card, drag on KPI Total Quantity
- Format the card to merge it with Total Quantity Card


<br>
<br>


## SECTION B: CHARTS REQUIREMENTS

### 1. Calender Heat Map

- Create a Week Number Column
```DAX
	WeekNumber = WEEKNUM(DateTable[Date],2)
```

- Use a Matrix chart in the Visualization panel
- Add weeknumber to Rows, weekday to Columns and Days to Values section
- Format and Sort weekday with column weekdaynum

- Add a new page for Tool tip, configure the tool tip
- Connect the tooltip to the calenda heat map

<BR>

### 2. Sales Analysis by Weekdays and Weekends

- Create a weekday_weekend column
```DAX
	Weekday_Weekend = IF(WEEKDAY(DateTable[Date], 2) <= 5, "Weekday", "Weekend")
```

- Create a weekday_weekend column
```DAX
	Weekday_Weekend = IF(WEEKDAY(DateTable[Date], 2) <= 5, "Weekday", "Weekend")
```

- Add a donut chart for visual

- Drag the Weekday_Weekend column into legend, total sales into values

- Format and Style the chart


<br>

### 3. Sales Analysis by Store Location

- Add clustered bar chart; store_location on y-axis: and total sales on x-axis
- Add a new value to x-axis to reflex the label, using new measure
```DAX
	Placeholder = 0
```
- Drag the Placeholder into x-axis, above total sales
- Format and style the chart

- create a new measure Store Location Label
```DAX
	Store Location Label = SELECTEDVALUE(Transactions[store_location]) & " | " & FORMAT([Total Sales]/1000, "$0.00K")
```

- Add "Label For Store Location" to the Placeholder' values in the Data Label section
- Add "KPI Total Sales" to the Total Sales' values in the Data Label section section to reflect MoM difference
- Sort Total Sales by decending order

<br>

### 4. Daily Sales Analysis with Average Line

- Create a new measure to calculate the Average Daily Sales for the Selected Month
```DAX
	AVG Daily Sales = 
	VAR TotalSales = [Total Sales] 
	VAR TotalDays = DISTINCTCOUNT('DateTable'[Date]) 
	RETURN 
	IF(TotalDays > 0, TotalSales / TotalDays, BLANK())
```

- Add a line chart and format it.

- "Go to Add futher analyses to your visual"; add a Y-axis consant line and name to "AVG Sales" 
- Under Line; click value to add condition "avg daily sales"


<br>

### 5. Sales Analysis by Product Category

- Add clustered bar chart; product category on y-axis and total sales on x-axis
- Add the previously created measure "Placeholder" to x-axis to reflex the label
- Drag the Placeholder into x-axis, above total sales
- Format and style the chart

- create a new measure Product Category Label
```DAX
	Product Category Label = 
	SELECTEDVALUE(Transactions[product_category]) & " | " & FORMAT([Total Sales]/1000, "$0.00K")
```

- In Data Label; under Series select placeholder, In value field set the condition to "Product Category Label"

  
<br>

### 6. Top 10 Products by Sales

- Just like Sales Analysis by Product Type, Add clustered bar chart
- Product Type on y-axis, Total sales and Placeholder on x-axis
- Format and style the chart

- create a new measure Product Type Label
```DAX
	Product Type Label = 
	SELECTEDVALUE(Transactions[product_type]) & " | " & FORMAT([Total Sales]/1000, "$0.00K")
```

- In Data Label; under Series select placeholder, In value field set the condition to "Product Type Label"


<br>

### 7. Sales Analysis by Days and Hours

- Use a Matrix chart in the Visualization panel
- Create a new column "Transaction Hour", to extract Hours from Transaction Time
- Add Transaction Hour to Rows, Weekday to Columns and Total Sales to Values section
- Format the chart
- Apply a condition to the Background color in the Cell Element section
- Apply a condition to the Font color, apply the same color code used in 'background color"
- Add a new page for "Tool Tip for Days/Hours", configure the tool tip
- Connect the tooltip, search for "tooltip" under Format your visual
- Turn On, go to "Page" select "Tool Tip for Days/Hours"

<br>

### 8. Arrange your Visuals Cards
[Dashboard Image](https://drive.google.com/file/d/1Lp54t17TotEAoqDZ3F7cke-5LcovXX0r/view?usp=sharing)
<br>
<br>

### 9. Visual Presentation
[**View**](https://drive.google.com/file/d/1tw2MqT5o5_MVQkP7Q-Vj_TrTh14wF6ri/view?usp=sharing)


<BR>

<br>
