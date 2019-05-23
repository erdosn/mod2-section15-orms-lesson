
### Questions
* What is an ORM? 
    * Object Relational Mapping
    * Taking an object and making it more robust
        * sqllite connection
* lots of moving parts, creating an engine, etc.
* wtf is an engine???????
* lambda functions...sure we can use this

### Objectives
YWBAT 
* build an orm for the northwind database
* explain why orms are useful

### Outline
* Questions
* Breakdown the structure of our ORM
* Build the Parent Class
* Building Child Classes
    * Assigning functions/attributes to parent and child


```python
import pandas as pd
import numpy as np
import sqlite3

import matplotlib.pyplot as plt
```

### Build Northwind Class - Parent


```python
class Northwind():
    """
    Northwind DB Object (sqlite object)
    Attributes
    - connection
    - cursor
    
    Methods
    - list_tables
    - list_relationships
    """
    
    def __init__(self, filename=None):
        self.conn = sqlite3.connect(filename)
        self.cursor = self.conn.cursor()
        
    
    def list_table_names(self):
        query = "select name from sqlite_master where type='table';"
        result = self.cursor.execute(query).fetchall()
        tables = []
        for res in result:
            tables.append(res[0])
        return tables
    
    
    
    def has_table_name(self):
        return 'table_name' in vars(self).keys()
    
    
    
    def get_column_list(self):
        columns = None
        if self.has_table_name():
            self.cursor.execute("select * from {} limit 1;".format(self.table_name)).fetchall()
            columns = []
            desc = self.cursor.description
            for d in desc:
                columns.append(d[0])
        return columns
    
    
    def select_all(self, table_name=None):
        res = None
        if self.has_table_name():
            
            if table_name is None: 
                query = 'select * from {}'.format(self.table_name)
                res = self.cursor.execute(query).fetchall()
                return res
            
            else:
                query = 'select * from {}'.format(table_name)
                res = self.cursor.execute(query).fetchall()
                return res
            
    
    def load_table_to_pandas(self, table_name=None):
        if not self.has_table_name():
            return None
        
        
        df = pd.read_sql("select * from {}".format(self.table_name), self.conn)
        return df
```

### Build Employee Class - Child


```python
class Employee(Northwind):
    
    def __init__(self):
        super().__init__(filename="Northwind_small.sqlite")  
        self.table_name = 'Employee'
        
        
    
```


```python
em = Employee()
```


```python
em.load_table_to_pandas()
```




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
      <th>Id</th>
      <th>LastName</th>
      <th>FirstName</th>
      <th>Title</th>
      <th>TitleOfCourtesy</th>
      <th>BirthDate</th>
      <th>HireDate</th>
      <th>Address</th>
      <th>City</th>
      <th>Region</th>
      <th>PostalCode</th>
      <th>Country</th>
      <th>HomePhone</th>
      <th>Extension</th>
      <th>Photo</th>
      <th>Notes</th>
      <th>ReportsTo</th>
      <th>PhotoPath</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Davolio</td>
      <td>Nancy</td>
      <td>Sales Representative</td>
      <td>Ms.</td>
      <td>1980-12-08</td>
      <td>2024-05-01</td>
      <td>507 - 20th Ave. E. Apt. 2A</td>
      <td>Seattle</td>
      <td>North America</td>
      <td>98122</td>
      <td>USA</td>
      <td>(206) 555-9857</td>
      <td>5467</td>
      <td>None</td>
      <td>Education includes a BA in psychology from Col...</td>
      <td>2.0</td>
      <td>http://accweb/emmployees/davolio.bmp</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Fuller</td>
      <td>Andrew</td>
      <td>Vice President, Sales</td>
      <td>Dr.</td>
      <td>1984-02-19</td>
      <td>2024-08-14</td>
      <td>908 W. Capital Way</td>
      <td>Tacoma</td>
      <td>North America</td>
      <td>98401</td>
      <td>USA</td>
      <td>(206) 555-9482</td>
      <td>3457</td>
      <td>None</td>
      <td>Andrew received his BTS commercial in 1974 and...</td>
      <td>NaN</td>
      <td>http://accweb/emmployees/fuller.bmp</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Leverling</td>
      <td>Janet</td>
      <td>Sales Representative</td>
      <td>Ms.</td>
      <td>1995-08-30</td>
      <td>2024-04-01</td>
      <td>722 Moss Bay Blvd.</td>
      <td>Kirkland</td>
      <td>North America</td>
      <td>98033</td>
      <td>USA</td>
      <td>(206) 555-3412</td>
      <td>3355</td>
      <td>None</td>
      <td>Janet has a BS degree in chemistry from Boston...</td>
      <td>2.0</td>
      <td>http://accweb/emmployees/leverling.bmp</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Peacock</td>
      <td>Margaret</td>
      <td>Sales Representative</td>
      <td>Mrs.</td>
      <td>1969-09-19</td>
      <td>2025-05-03</td>
      <td>4110 Old Redmond Rd.</td>
      <td>Redmond</td>
      <td>North America</td>
      <td>98052</td>
      <td>USA</td>
      <td>(206) 555-8122</td>
      <td>5176</td>
      <td>None</td>
      <td>Margaret holds a BA in English literature from...</td>
      <td>2.0</td>
      <td>http://accweb/emmployees/peacock.bmp</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Buchanan</td>
      <td>Steven</td>
      <td>Sales Manager</td>
      <td>Mr.</td>
      <td>1987-03-04</td>
      <td>2025-10-17</td>
      <td>14 Garrett Hill</td>
      <td>London</td>
      <td>British Isles</td>
      <td>SW1 8JR</td>
      <td>UK</td>
      <td>(71) 555-4848</td>
      <td>3453</td>
      <td>None</td>
      <td>Steven Buchanan graduated from St. Andrews Uni...</td>
      <td>2.0</td>
      <td>http://accweb/emmployees/buchanan.bmp</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Suyama</td>
      <td>Michael</td>
      <td>Sales Representative</td>
      <td>Mr.</td>
      <td>1995-07-02</td>
      <td>2025-10-17</td>
      <td>Coventry House Miner Rd.</td>
      <td>London</td>
      <td>British Isles</td>
      <td>EC2 7JR</td>
      <td>UK</td>
      <td>(71) 555-7773</td>
      <td>428</td>
      <td>None</td>
      <td>Michael is a graduate of Sussex University (MA...</td>
      <td>5.0</td>
      <td>http://accweb/emmployees/davolio.bmp</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>King</td>
      <td>Robert</td>
      <td>Sales Representative</td>
      <td>Mr.</td>
      <td>1992-05-29</td>
      <td>2026-01-02</td>
      <td>Edgeham Hollow Winchester Way</td>
      <td>London</td>
      <td>British Isles</td>
      <td>RG1 9SP</td>
      <td>UK</td>
      <td>(71) 555-5598</td>
      <td>465</td>
      <td>None</td>
      <td>Robert King served in the Peace Corps and trav...</td>
      <td>5.0</td>
      <td>http://accweb/emmployees/davolio.bmp</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Callahan</td>
      <td>Laura</td>
      <td>Inside Sales Coordinator</td>
      <td>Ms.</td>
      <td>1990-01-09</td>
      <td>2026-03-05</td>
      <td>4726 - 11th Ave. N.E.</td>
      <td>Seattle</td>
      <td>North America</td>
      <td>98105</td>
      <td>USA</td>
      <td>(206) 555-1189</td>
      <td>2344</td>
      <td>None</td>
      <td>Laura received a BA in psychology from the Uni...</td>
      <td>2.0</td>
      <td>http://accweb/emmployees/davolio.bmp</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Dodsworth</td>
      <td>Anne</td>
      <td>Sales Representative</td>
      <td>Ms.</td>
      <td>1998-01-27</td>
      <td>2026-11-15</td>
      <td>7 Houndstooth Rd.</td>
      <td>London</td>
      <td>British Isles</td>
      <td>WG2 7LT</td>
      <td>UK</td>
      <td>(71) 555-4444</td>
      <td>452</td>
      <td>None</td>
      <td>Anne has a BA degree in English from St. Lawre...</td>
      <td>5.0</td>
      <td>http://accweb/emmployees/davolio.bmp</td>
    </tr>
  </tbody>
</table>
</div>




```python
nw = Northwind("Northwind_small.sqlite")
```


```python
nw.list_table_names()
```




    ['Employee',
     'Category',
     'Customer',
     'Shipper',
     'Supplier',
     'Order',
     'Product',
     'OrderDetail',
     'CustomerCustomerDemo',
     'CustomerDemographic',
     'Region',
     'Territory',
     'EmployeeTerritory']



### Assessment
