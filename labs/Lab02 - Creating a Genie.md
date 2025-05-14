<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_genie.png" width=900>

# Hands-On Lab 02 - Creating a Genie

Hands-on training on the Databricks platform with a focus on question and answer features using natural language.

</br></br>

## Exercise Objectives

The objective of this lab is to use the AI/BI Genie to enable sales data analysis using only **English**.

</br></br>

## Exercise 03.01 - Create AI/BI Genie

Let's start by creating a Genie to ask our questions. To do this, follow the steps below:

1. In the main menu (on the left), click on `New` > `Genie space`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_01.png"><br><br>

2. Configure your Genie
    - Create a name for your Genie, for example `<your initials> Sales Genie`
    - Select your SQL Warehouse
    - Select the following tables:
        - sales
        - inventory
        - dim_product
        - dim_store
    - Click on `Save`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_02.png" width=800>

</br></br>

## Exercise 03.02 - Asking Questions to AI/BI Genie

With our Genie ready, we can start our analysis!

Just use the chat to ask the questions below:

- What is the revenue in Oct/22?
- Now, break it down by product
- Keep only the top 10 products with the highest revenue
- Create a bar chart
- What is the total number of products sold in generics?
- What is the total value sold of anxiolytics?
- Which products had a sales-to-inventory ratio greater than 0.8 in October 2022?

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_05.png"><br><br>

Notice that, even with very little context, the Genie was able to:
- Infer which tables and columns are relevant to answer our questions
- Apply filters and aggregations
- Answer additional questions about a previous answer
- Understand how to use jargon
- Combine different tables
- Calculate derived metrics

Take some time to explore and ask additional questions!

</br></br>

## Exercise 03.03 - Using Comments

Even so, there may be scenarios where we need to provide additional context to the Genie to get more accurate answers.

Now, let's explore some ways to help the Genie when we identify a need.

The first one is to document our tables. All comments we add to the tables are used by the Genie to better understand what that data is.

Let's see how it works:

1. Ask the following question:
    - What is the total sales value per store? Show the store name

2. Oops, it seems like the Genie couldn't figure out which column contains the store name. Use the SQL Editor to add a comment to the **nlj** column of the **dim_store** table and explain that it contains that information
    - `ALTER TABLE dim_store ALTER COLUMN nlj COMMENT 'Store name'`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_06.png"><br><br>

3. Ask the previous question again<br><br>

Notice that this time the Genie used the correct column that contains the store name.

Documenting our tables with comments is always a good practice! This helps with understanding, discovery, and reuse of that data by other people. Additionally, it helps improve the Genie's answers.

However, our query still didn't return any results. Let's look for a solution!

</br></br>

## Exercise 03.04 - Using Primary Keys

Apparently, the **id_loja** column of the **dim_store** table is not the best field to join with the sales table. In fact, the correct column is **cod**!

Let's add primary and foreign keys to those tables so that the Genie doesn't have to infer how to join them!

1. Use the SQL Editor to add primary and foreign keys to the `dim_store` and `sales` tables

``` sql
ALTER TABLE dim_store ALTER COLUMN cod SET NOT NULL;
ALTER TABLE dim_store ADD CONSTRAINT pk_dim_store PRIMARY KEY (cod);
ALTER TABLE sales ADD CONSTRAINT fk_sales_dim_store FOREIGN KEY (id_loja) REFERENCES dim_store(cod);
```
2. Ask the previous question again in the Genie

Done! With this information, the Genie can now answer our question correctly!

</br></br>

## Exercise 03.05 - Using Instructions

As we saw, the Genie uses all the documentation of our tables to answer our questions. However, due to security reasons, it doesn't have access to the data itself!

That's why, to complement the knowledge the Genie already has about our databases, we can also create instructions!

**Instructions** allow us to save and parameterize complex logic within our catalog to be reused by other people and/or other queries in a simple way – even outside of the Genie.

In our context, instructions will also work as validated and certified tools by the responsible teams that the Genie can decide to use in its answers.

Let's see it in practice:

1. Ask the question:
    - Calculate the number of items sold for prescription

2. Actually, we don't have enough information in our database to answer that question! To do that, add the following instruction:
    - `* to calculate indicators on prescription use categoria_regulatoria <> 'GENÉRICO'`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_07.png">

3. Ask the previous question again

Done! Now the Genie can answer questions about prescriptions too!

</br></br>

## (Optional) Exercise 03.06 - Using Example Queries

In some cases, we need to perform complex joins and calculations to answer our questions, and the Genie may not understand how to put together all the necessary reasoning.

In those cases, we can provide example queries validated and certified by the responsible teams. This is also an interesting mechanism to ensure the accuracy of the answers.

Let's see how it works:

1. Ask the question:
    - Calculate the number of items sold by a 3-month moving window

2. Here the Genie already performed a sum in a moving window, but it didn't exactly match what we wanted. So, add an example query following the steps below:
    - Click on `Instructions` in the left menu
    - Click on `Add Example Query`
    - In the top field, enter the previous question
    - In the bottom field, enter the SQL query
        - `SELECT window.end AS dt_venda, SUM(vl_venda) FROM sales GROUP BY WINDOW(dt_venda, '90 days', '1 day')`

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_08.png">

3. Ask the previous question again

Notice that now the Genie was able to answer our question correctly!

</br></br>

## (Optional) Exercise 03.07 - Using Functions

Another resource we can use to help the Genie with complex calculations are functions!

**Functions** allow us to save and parameterize complex logic within our catalog to be reused by other people and/or other queries in a simple way – even outside of the Genie.

In our context, functions will also work as validated and certified tools by the responsible teams that the Genie can decide to use in its answers.

Let's see it in practice:

1. Ask the question:
    - What is the projected profit of AAS?

2. Actually, we don't have enough information in our database to answer that question! To do that, create the function below with the logic to calculate the average projected profit of a product:

``` sql
CREATE OR REPLACE FUNCTION calc_profit(medicine STRING)
  RETURNS TABLE(nome_medicamento STRING, projected_profit DOUBLE)
  COMMENT 'Use this function to calculate the projected profit of a medicine'
  RETURN 
    SELECT
      m.nome_medicamento as medicine,
      sum(case when m.categoria_regulatoria == 'GENÉRICO' then 1 else 0.5 end * v.vl_venda) / sum(v.qt_venda) as projected_profit
    FROM sales v
    LEFT JOIN dim_product m
    ON v.id_produto = m.id_produto
    WHERE m.nome_medicamento = calc_profit.medicine
    GROUP BY ALL
```

3. Add this function to your Genie

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/genie_09.png">

4. Ask the previous question again

Done! With that, we were able to calculate the average profit of our product!

<br><br>

# Congratulations!

You have completed the **AI/BI Genie** lab!

Now, you know how to use the Genie to analyze your data using only natural language – in addition to the main resources to refine its accuracy and answer more complex questions!