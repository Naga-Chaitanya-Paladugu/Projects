# Installation & Importing Libraries


```python
# Install the pyodbc library to allow Python to interface with ODBC databases (such as SQL Server)
!pip install pyodbc

# Import the pyodbc library 
import pyodbc

# Import the pandas library for data manipulation and analysis
import pandas as pd
```

    Requirement already satisfied: pyodbc in c:\users\palad\anaconda3\lib\site-packages (5.0.1)


# Setting Up Connection


```python
# Establish a connection to the SQL Server database using pyodbc
conn = pyodbc.connect(
    'Driver={ODBC Driver 17 for SQL Server};'  # Specifies the ODBC driver for SQL Server
    'Server=Chaitanya;'  # Specifies the name of the server (replace with actual server name if needed)
    'Database=SalesData;'  # Specifies the name of the database to connect to
    'Trusted_Connection=yes;'  # Uses Windows Authentication for the connection
)

# Create a cursor object
cursor = conn.cursor()

# Execute a query to check the connection
cursor.execute("SELECT @@version;")

# Fetch and display the result
row = cursor.fetchone()
while row:
    print(row[0])
    row = cursor.fetchone()
```

    Microsoft SQL Server 2022 (RTM) - 16.0.1000.6 (X64) 
    	Oct  8 2022 05:58:25 
    	Copyright (C) 2022 Microsoft Corporation
    	Developer Edition (64-bit) on Windows 10 Pro 10.0 <X64> (Build 22631: )
    


# Inserting Records


```python
# Insert records into the SalesPerson table
salespeople = [
    ('SP026', 'Liam', 'Ford'),
    ('SP027', 'Noah', 'Smith'),
    ('SP028', 'Oliver', 'Johnson'),
    ('SP029', 'Elijah', 'Williams'),
    ('SP030', 'William', 'Brown'),
    ('SP031', 'James', 'Jones'),
    ('SP032', 'Benjamin', 'Garcia'),
    ('SP033', 'Lucas', 'Miller'),
    ('SP034', 'Henry', 'Davis'),
    ('SP035', 'Alexander', 'Rodriguez'),
    ('SP036', 'Mason', 'Martinez'),
    ('SP037', 'Michael', 'Hernandez'),
    ('SP038', 'Ethan', 'Lopez'),
    ('SP039', 'Daniel', 'Wilson'),
    ('SP040', 'Jacob', 'Anderson'),
    ('SP041', 'Logan', 'Thomas'),
    ('SP042', 'Jackson', 'Taylor'),
    ('SP043', 'Levi', 'Moore'),
    ('SP044', 'Sebastian', 'Jackson'),
    ('SP045', 'Mateo', 'Martin'),
    ('SP046', 'Jack', 'Lee'),
    ('SP047', 'Owen', 'Perez'),
    ('SP048', 'Theodore', 'Thompson'),
    ('SP049', 'Aiden', 'White'),
    ('SP050', 'Samuel', 'Harris')
]

cursor.executemany("INSERT INTO SalesPerson (SalesPersonID, FirstName, LastName) VALUES (?, ?, ?)", salespeople)
conn.commit()
```


```python
# Insert records into the Customer table
customers = [
    ('C026', 'Zoe Carter', 'CA'),
    ('C027', 'Sophia Lee', 'NY'),
    ('C028', 'Isabella Harris', 'TX'),
    ('C029', 'Mia Clark', 'FL'),
    ('C030', 'Amelia Lewis', 'IL'),
    ('C031', 'Harper Walker', 'PA'),
    ('C032', 'Evelyn Young', 'OH'),
    ('C033', 'Abigail Allen', 'MI'),
    ('C034', 'Emily Wright', 'AZ'),
    ('C035', 'Ella Scott', 'GA'),
    ('C036', 'Madison Torres', 'WA'),
    ('C037', 'Scarlett Nguyen', 'VA'),
    ('C038', 'Victoria Hill', 'MA'),
    ('C039', 'Aria Moore', 'CO'),
    ('C040', 'Grace Rivera', 'MD'),
    ('C041', 'Chloe Jackson', 'NC'),
    ('C042', 'Camila Martin', 'NJ'),
    ('C043', 'Penelope Lee', 'NV'),
    ('C044', 'Riley Hernandez', 'WI'),
    ('C045', 'Layla Lopez', 'MN'),
    ('C046', 'Lillian Roberts', 'CT'),
    ('C047', 'Nora Sanchez', 'MO'),
    ('C048', 'Zoey Miller', 'TN'),
    ('C049', 'Mila Davis', 'IN'),
    ('C050', 'Aubrey Garcia', 'UT')
]

cursor.executemany("INSERT INTO Customer (CustomerID, CustomerName, CustomerState) VALUES (?, ?, ?)", customers)
conn.commit()
```


```python
# Insert records into the Orders table
from datetime import datetime, timedelta

# Creating date range
start_date = datetime.strptime('2024-01-26', '%Y-%m-%d')
order_dates = [(start_date + timedelta(days=i)).strftime('%Y-%m-%d') for i in range(25)]

orders = [(f'O0{26 + i}', f'C0{26 + i}', f'SP0{26 + i}', order_dates[i]) for i in range(25)]

cursor.executemany("INSERT INTO Orders (OrderNumber, CustomerID, SalesPersonID, OrderDate) VALUES (?, ?, ?, ?)", orders)
conn.commit()
```


```python
# Insert records into the Items table
items = [
    ('I026', 'Product Z', 140.00),
    ('I027', 'Product Y', 145.00),
    ('I028', 'Product X', 150.00),
    ('I029', 'Product W', 155.00),
    ('I030', 'Product V', 160.00),
    ('I031', 'Product U', 165.00),
    ('I032', 'Product T', 170.00),
    ('I033', 'Product S', 175.00),
    ('I034', 'Product R', 180.00),
    ('I035', 'Product Q', 185.00),
    ('I036', 'Product P', 190.00),
    ('I037', 'Product O', 195.00),
    ('I038', 'Product N', 200.00),
    ('I039', 'Product M', 205.00),
    ('I040', 'Product L', 210.00),
    ('I041', 'Product K', 215.00),
    ('I042', 'Product J', 220.00),
    ('I043', 'Product I', 225.00),
    ('I044', 'Product H', 230.00),
    ('I045', 'Product G', 235.00),
    ('I046', 'Product F', 240.00),
    ('I047', 'Product E', 245.00),
    ('I048', 'Product D', 250.00),
    ('I049', 'Product C', 255.00),
    ('I050', 'Product B', 260.00)
]

cursor.executemany("INSERT INTO Items (ItemID, Item, ItemPrice) VALUES (?, ?, ?)", items)
conn.commit()
```


```python
# Insert records into the OrderDetails table
order_details = [
    ('D026', 'O026', 'I026', 2),
    ('D027', 'O027', 'I027', 1),
    ('D028', 'O028', 'I028', 3),
    ('D029', 'O029', 'I029', 4),
    ('D030', 'O030', 'I030', 1),
    ('D031', 'O031', 'I031', 2),
    ('D032', 'O032', 'I032', 3),
    ('D033', 'O033', 'I033', 4),
    ('D034', 'O034', 'I034', 5),
    ('D035', 'O035', 'I035', 6),
    ('D036', 'O036', 'I036', 7),
    ('D037', 'O037', 'I037', 8),
    ('D038', 'O038', 'I038', 9),
    ('D039', 'O039', 'I039', 10),
    ('D040', 'O040', 'I040', 2),
    ('D041', 'O041', 'I041', 3),
    ('D042', 'O042', 'I042', 4),
    ('D043', 'O043', 'I043', 5),
    ('D044', 'O044', 'I044', 6),
    ('D045', 'O045', 'I045', 7),
    ('D046', 'O046', 'I046', 8),
    ('D047', 'O047', 'I047', 9),
    ('D048', 'O048', 'I048', 10),
    ('D049', 'O049', 'I049', 1),
    ('D050', 'O050', 'I050', 2)
]

cursor.executemany("INSERT INTO OrderDetails (OrderDetailID, OrderNumber, ItemID, ItemQuantity) VALUES (?, ?, ?, ?)", order_details)
conn.commit()
```

# Visualizations


```python
import pandas as pd
import matplotlib.pyplot as plt
```


```python
# SQL query to fetch sales by customer including those with no sales
query_customer_sales = """
SELECT c.CustomerName, ISNULL(SUM(od.ItemQuantity * i.ItemPrice), 0) AS TotalSales
FROM Customer c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID
LEFT JOIN OrderDetails od ON o.OrderNumber = od.OrderNumber
LEFT JOIN Items i ON od.ItemID = i.ItemID
GROUP BY c.CustomerName
ORDER BY TotalSales DESC;
"""

# Execute the query and store results in a DataFrame
df_customer_sales = pd.read_sql_query(query_customer_sales, conn)

# Display the DataFrame as a table to ensure all customers are included
print("Sales by Customer Table (Including Customers with No Sales):")
df_customer_sales
```

    Sales by Customer Table (Including Customers with No Sales):


    C:\Users\palad\AppData\Local\Temp\ipykernel_21936\2579167737.py:13: UserWarning: pandas only supports SQLAlchemy connectable (engine/connection) or database string URI or sqlite3 DBAPI2 connection. Other DBAPI2 objects are not tested. Please consider using SQLAlchemy.
      df_customer_sales = pd.read_sql_query(query_customer_sales, conn)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CustomerName</th>
      <th>TotalSales</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Zoey Miller</td>
      <td>2500.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nora Sanchez</td>
      <td>2205.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Aria Moore</td>
      <td>2050.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kevin Hayes</td>
      <td>2000.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Lillian Roberts</td>
      <td>1920.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Victoria Hill</td>
      <td>1800.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Layla Lopez</td>
      <td>1645.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Scarlett Nguyen</td>
      <td>1560.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Grace Cox</td>
      <td>1475.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Julia Torres</td>
      <td>1400.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Linda Hill</td>
      <td>1380.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Riley Hernandez</td>
      <td>1380.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Madison Torres</td>
      <td>1330.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Penelope Lee</td>
      <td>1125.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Ella Scott</td>
      <td>1110.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Frank Moore</td>
      <td>1015.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Emily Wright</td>
      <td>900.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Camila Martin</td>
      <td>880.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Isaac Scott</td>
      <td>880.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Abigail Allen</td>
      <td>700.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Chloe Jackson</td>
      <td>645.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Erica Smith</td>
      <td>635.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Mia Clark</td>
      <td>620.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Aubrey Garcia</td>
      <td>520.0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Evelyn Young</td>
      <td>510.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Isabella Harris</td>
      <td>450.0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Harry Wright</td>
      <td>440.0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Grace Rivera</td>
      <td>420.0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Harper Walker</td>
      <td>330.0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>David Lee</td>
      <td>320.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Zoe Carter</td>
      <td>280.0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Nancy Martinez</td>
      <td>270.0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Mila Davis</td>
      <td>255.0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Bob Johnson</td>
      <td>225.0</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Amelia Lewis</td>
      <td>160.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Sophia Lee</td>
      <td>145.0</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Carol King</td>
      <td>85.0</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Alice Brown</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Quincy Adams</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Rachel Diaz</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Sam Foster</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Oscar Nunez</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Patty Evans</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Tina Alexander</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Ursula Jennings</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Victor Bryan</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Wendy Armstrong</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Xavier Bush</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Yvonne Charles</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Zachary Dean</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Query to fetch sales by customer
query_customer_sales = """
SELECT c.CustomerName, SUM(od.ItemQuantity * i.ItemPrice) AS TotalSales
FROM Customer c
JOIN Orders o ON c.CustomerID = o.CustomerID
JOIN OrderDetails od ON o.OrderNumber = od.OrderNumber
JOIN Items i ON od.ItemID = i.ItemID
GROUP BY c.CustomerName
ORDER BY TotalSales DESC;
"""

# Execute the query and store results in a DataFrame
df_customer_sales = pd.read_sql_query(query_customer_sales, conn)

# Plotting sales by customer
plt.figure(figsize=(10, 7))  # Increased figure size
plt.barh(df_customer_sales['CustomerName'], df_customer_sales['TotalSales'], color='skyblue')
plt.xlabel('Total Sales ($)')
plt.ylabel('Customer')
plt.title('Sales by Customer for All Time')
plt.gca().invert_yaxis()  # Invert y-axis to show the highest sales at the top
plt.show()
```

    C:\Users\palad\AppData\Local\Temp\ipykernel_21936\3114822517.py:13: UserWarning: pandas only supports SQLAlchemy connectable (engine/connection) or database string URI or sqlite3 DBAPI2 connection. Other DBAPI2 objects are not tested. Please consider using SQLAlchemy.
      df_customer_sales = pd.read_sql_query(query_customer_sales, conn)



    
![png](output_13_1.png)
    



```python
# SQL query to fetch sales by state
query_state_sales = """
SELECT c.CustomerState, SUM(od.ItemQuantity * i.ItemPrice) AS TotalSales
FROM Customer c
JOIN Orders o ON c.CustomerID = o.CustomerID
JOIN OrderDetails od ON o.OrderNumber = od.OrderNumber
JOIN Items i ON od.ItemID = i.ItemID
GROUP BY c.CustomerState
ORDER BY TotalSales DESC;
"""

# Execute the query and store results in a DataFrame
df_state_sales = pd.read_sql_query(query_state_sales, conn)

# Plotting sales by state
plt.figure(figsize=(10, 4))  # Adjusted figure size for readability
plt.bar(df_state_sales['CustomerState'], df_state_sales['TotalSales'], color='lightgreen')
plt.xlabel('State')
plt.ylabel('Total Sales ($)')
plt.title('Sales by State for All Time')
plt.xticks(rotation=45)  # Rotate state labels for better visibility
plt.tight_layout()  # Adjust layout to fit elements
plt.show()
```

    C:\Users\palad\AppData\Local\Temp\ipykernel_21936\3899835261.py:13: UserWarning: pandas only supports SQLAlchemy connectable (engine/connection) or database string URI or sqlite3 DBAPI2 connection. Other DBAPI2 objects are not tested. Please consider using SQLAlchemy.
      df_state_sales = pd.read_sql_query(query_state_sales, conn)



    
![png](output_14_1.png)
    



```python

```
