<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_genie.png" width=900>

# Hands-On Lab 01 - Creating a Dashboard

Practical training on the Databricks platform with a focus on Exploratory Analysis and Dashboard functionalities.
</br></br>

## Exercise Objectives

The objective of this lab is to create a Dashboard using the stock data of Big Tech companies from NASDAQ.
</br></br></br>

## Exercise 02.01 - Step-by-Step Dashboard Creation

Go to the "**Catalog**" option in the Databricks sidebar, </br>
filter the "**academy**" catalog and "**genie_aibi**" schema, </br>
and select the "**stock_bigtech**" table. </br>
Click on the "**Open in a dashboard**" button.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_01.png" width="800px">
</br></br></br>

Databricks will generate a first version of the dashboard </br>
and open a window for creating a dashboard element. </br>
The first editable line is a **GenAI Assistant**, </br>
where we can describe, in natural language, what we want to create. </br>
Note that below the line, there are already some suggestions provided by the assistant.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_02.png" width="800px">
</br></br></br>

As we type in the editable line, options based on the table context will appear. </br>
Type the following phrase in the assistant line:
</br></br>
``` sql
chart of volume of shares per day and per company 
``` 
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_03.png" width="600px">
</br></br></br>

Let's improve the dashboard design and object layout. </br>
Drag the "Volume of Shares" chart to the center of the Dashboard. </br>
Make the chart width span the entire screen.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_04.png" width="800px">
</br>
</br>
</br>

## Exercise 02.02 - Second Chart Element

Let's create a new chart element. </br>
In the blue floating menu at the bottom of the dashboard, </br>
click on the second button (with the chart icon).
</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_botao.png" width="200px">

</br></br></br>

In the assistant editable line, type:
</br>
``` sql
line chart of closing value per day and per company

``` 
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_05.png" width="800px">
</br></br></br>

## Exercise 02.03 - Adding a Filter

Rearrange the dashboard design, </br>
aligning the new chart with the "Volumes" chart. </br>
Then click on the blue floating menu on the FILTER icon. </br>
Choose the attribute (Field): "**company**"
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_06.png" width="850px">
</br></br></br>

## Exercise 02.04 - Adding an Image to the Dashboard

In the dashboard title text box, delete the previous text, </br>
and replace it with the following markdown code: </br>
</br>

``` sql
![image](https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_painel.png)
```

</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_07.png" width="700px">
</br></br></br>

The Dashboard will look like this. </br>
Make the necessary chart alignment adjustments. </br>
Change the Dashboard name in the top bar. </br>
Click on the "**Publish**" button to publish the Dashboard.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_08.png" width="700px">
</br></br></br>

## (Optional) Exercise 02.05 - Creating a New Data Context

Let's create a new data context. </br>
To do this, go to the "**SQL Editor**" option in the Databricks sidebar, </br>
select the Catalog and Schema in the Query Editor top bar, </br>
and type the following text:
``` 

Select the company name, stock,
minimum closing value, maximum closing value
and percentage variation between the minimum and maximum closing value
from the stock_bigtech table
grouping by company and stock

```
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_09.png" width="700px">
</br></br></br>

Add to the result the concatenation line with the stock name (STOCK), </br>
with the LINK (URL) of an image. </br>

``` sql

SELECT 
  "https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/" || stock || ".png" AS image,
  company,
  stock,
  MIN(close) AS min_close,
  MAX(close) AS max_close,
  ((MAX(close) - MIN(close)) / MIN(close) * 100) AS percentual_variacao
FROM luis_assuncao.genie_aibi.stock_bigtech
GROUP BY company, stock;

```
</br>
When running the query (RUN button), </br>
the expected result is shown below: </br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_10.png" width="700px">
</br></br></br>

## (Optional) Exercise 02.06 - Adding the New Data Context to the Dashboard

Now, let's add the new data context (calculated in the Query); </br>
as another data source in the Dashboard. </br>
To do this, go back to the "**Dashboards**" option in the Databricks sidebar, </br>
choose the name of the created Dashboard, </br>
and click on the "**Published**" button to switch to "**Draft**".
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_10b.png" width="700px">
</br></br></br>

Click on the "**Data**" option. </br>
In the "Create another Dataset" item, </br>
click on the "**+Create from SQL**" button.

</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_11.png" width="700px">
</br></br></br>

Copy and paste the SQL code generated in the previous exercise:

``` sql

SELECT 
  "https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/" || stock || ".png" AS image,
  company,
  stock,
  MIN(close) AS min_close,
  MAX(close) AS max_close,
  ((MAX(close) - MIN(close)) / MIN(close) * 100) AS percentual_variacao
FROM luis_assuncao.genie_aibi.stock_bigtech
GROUP BY company, stock;

```
And then click on the "**RUN**" button.
</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_12.png" width="700px">
</br></br></br>

## (Optional) Exercise 02.07 - Adding a New Chart with the New Data Context

Click on the blue floating menu at the bottom of the dashboard, </br>
on the button with the chart icon, </br>
and in the configuration bar (right side of the dashboard), </br>
choose the name of the dataset (which came from the SQL Query). 
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_13.png" width="700px">
</br></br></br>

Configure the Visualization type to "Table". </br>
Mark the option to include ALL fields in the table.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_14.png" width="700px">
</br></br></br>

In the configuration, click on the field (column) with the name "**image**". </br>
In the "Display" option, configure as "image". </br>

</br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_15.png" width="700px">
</br></br></br>

In the "SIZE" option, set the value to "25" in the "width" field. </br>
In the "Default column width" option, set the value to "150".
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_16.png" width="700px">
</br></br></br>

As the expected result, we will have the figure below. </br>
Save (Publish) the Dashboard again.
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab2_17.png" width="700px">
</br></br></br>