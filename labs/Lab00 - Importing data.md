<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/header_genie.png" width=900>

# Hands-On LAB 00 - Importing Data

Hands-on training on the Databricks platform focusing on Exploratory Analysis and Dashboard features.

## Exercise Objectives

The objective of this lab is to import the DATA that will be used in the exercises.
</br></br></br>

## Exercise 00.01 - Creating the Database

```sql
USE CATALOG academy;

CREATE DATABASE IF NOT EXISTS genie_aibi;
```

</br></br>

## Exercise 00.02 - Importing Files

In the GITHUB repository, in the DATA folder, click on each data file name (*.CSV and *.PARQUET), and click on the button to download the file locally, as shown in the figure below:
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab1_01.png" width=900>

</br></br></br>
In the Databricks MENU, click on the "+NEW" option and then on "Add or Upload Data":
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab1_02.png" width="400px">

</br></br></br>
Choose the "Create or Modify Table" option:
</br></br>
<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab1_03.png" width="700px">

</br></br></br>
Select the Catalog name (ACADEMY), the Schema name (GENIE_AIBI), and a name for the new table:

<img src="https://raw.githubusercontent.com/Databricks-BR/genie_ai_bi/main/images/lab1_04.png" width=900>
</br></br>
</br></br></br>