# Part1

#### Question-1 
SELECT * FROM Customers WHERE Country = 'UK';

#### Question-2

SELECT Customers.CustomerName, COUNT(OrderID)
FROM Orders
INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID GROUP BY CustomerName ORDER BY COUNT(OrderID)

**Ernst Handel - 10 Orders**

#### Question-3
SELECT Suppliers.SupplierName, AVG(Price)
FROM Suppliers
INNER JOIN Products ON Suppliers.SupplierID=Products.SupplierID GROUP BY SupplierName ORDER BY AVG(Price) DESC

#### Question-4
SELECT COUNT(DISTINCT(Country)) from Customers;



#### Question-5
SELECT
    cat.CategoryName, 
    COUNT(*) AS Count
FROM
    OrderDetails as ord
  JOIN
    Products as prod
  JOIN
    Categories AS cat
  ON
      ord.ProductID = prod.ProductID
    AND
      prod.CategoryID = cat.CategoryID
GROUP BY
    cat.CategoryID
ORDER BY
    Count DESC
LIMIT 1;



#### Question-6
SELECT
    ord.OrderID,
    SUM(ord.Quantity * prod.Price) as Total
FROM
    OrderDetails as ord
  JOIN
    Products as prod
  ON
    ord.ProductID = prod.ProductID
GROUP BY
    ord.OrderID
ORDER BY
    Total DESC;

       

#### Question-7
SELECT
    ord.EmployeeID,
    SUM(orddet.Quantity * prod.Price) as Total
FROM
    Orders as ord
  JOIN
    Products as prod
  ON
    ord.OrderID = orddet.OrderID
  JOIN
    OrderDetails as orddet
  ON
    orddet.ProductID = prod.ProductID 
GROUP BY
    ord.EmployeeID
ORDER BY
    Total DESC ;

#### Question-8
SELECT * FROM Employees
WHERE Notes LIKE '%BS%';



#### Question-9
SELECT
    SupplierName,
    COUNT(*) as NumProducts,
    AVG(prod.Price) as MeanPrice
FROM
    Suppliers AS sup
  JOIN
    Products AS prod
  ON
    sup.SupplierID = prod.SupplierID
GROUP BY
    sup.SupplierID
HAVING
    NumProducts >= 3
ORDER BY
    MeanPrice DESC;

# Part2
#### Connection setup required for the below SQLLite questions.
import pandas as pd
import sqlite3

conn = sqlite3.connect('lahman.sqlite')

query = "SELECT * FROM sqlite_master;"

df_schema = pd.read_sql_query(query, conn)


#### Question-1
query = "SELECT teamID,yearID,SUM(Salary) FROM salaries GROUP BY teamID,yearID ORDER BY teamID;"
df= pd.read_sql_query(query, conn)

#### Question-2
query = "SELECT playerID, min(yearID), max(yearID) from Fielding GROUP BY playerID;"
df = pd.read_sql_query(query, conn)

#### Question-3
query = "SELECT playerid, COUNT(*)
                        FROM allstarfull
                        GROUP BY 1
                        ORDER BY 2 DESC
                        LIMIT 1"
df = pd.read_sql_query(query, conn)


#### Question-4
query = "SELECT schoolid, count(distinct playerid)
                        FROM schoolsplayers
                        GROUP BY 1
                        ORDER BY 2 DESC
                        LIMIT 1"
df = pd.read_sql_query(query, conn)      

#### Question-5
query = "SELECT playerid, finalgame, debut, (finalgame-debut) AS days
                        FROM master
                        WHERE finalgame IS NOT NULL and debut IS NOT NULL
                        ORDER BY 4 DESC
                        LIMIT 5"
df = pd.read_sql_query(query, conn) 

#### Question-6
query = "SELECT EXTRACT(MONTH FROM debut) AS debut_month, COUNT(*)
                        FROM master
                        GROUP BY 1
                        ORDER BY 1 ASC"
df = pd.read_sql_query(query, conn) 

#### Question-7
query = "SELECT S.playerid, AVG(salary)
                        FROM Salaries S LEFT JOIN Master M
                        ON S.playerid = M.playerid
                        GROUP BY 1
                        LIMIT 5"
df = pd.read_sql_query(query, conn) 

# Part3

#### Question-1
query = "Select home_team_api_id, MAX(mycount) FROM (SELECT home_team_api_id, COUNT(home_team_goal) mycount from Match GROUP BY home_team_api_id);"

#### Question-2
No
query = "Select away_team_api_id, MAX(mycount) FROM (SELECT away_team_api_id, COUNT(away_team_goal) mycount from Match GROUP BY away_team_api_id);"

#### Question-3
query = "Select COUNT(match_api_id) FROM Match WHERE home_team_goal = away_team_goal;"

#### Question-4
Last Name = 18
query = "SELECT COUNT(*) from Player WHERE player_name LIKE '%Smith';"

Anywhere in the name = 18
query = "SELECT COUNT(*) from Player WHERE player_name LIKE '%Smith%';"

#### Question-5



#### Question-6
Right Percentage = 75.231278

query_subset_right = """
    SELECT 
    Count(id) as \'R_Total\'
    FROM Player_Attributes  
    WHERE preferred_foot = \'right\';
"""

query_subset_total = """
    SELECT 
    Count(id) as \'Total\'
    FROM Player_Attributes; 
"""

df_right = pd.read_sql_query(query_subset_right, conn)
df_total = pd.read_sql_query(query_subset_total, conn)
perc = (df_right['R_Total']/df_total['Total'])*100

# Part4
Creating the below view for the questions as part of Part-4.
target_query = """
CREATE VIEW allMatches AS
SELECT *, 'US Open' AS tournament, 'women' as gender FROM us_women
UNION ALL
SELECT *, 'AUST Open' AS tournament, 'women' as gender FROM aus_women
UNION ALL
SELECT *, 'French Open' AS tournament, 'women' as gender FROM french_women
UNION ALL
SELECT *, 'Wimbledon' AS tournament, 'women' as gender FROM wimb_women
UNION ALL
SELECT *, 'US Open' AS tournament, 'men' as gender FROM us_men
UNION ALL
SELECT *, 'AUST Open' AS tournament, 'men' as gender FROM aus_men
UNION ALL
SELECT *, 'French Open' AS tournament, 'men' as gender FROM french_men
UNION ALL
SELECT *, 'Wimbledon' AS tournament, 'men' as gender FROM wimb_men
"""

#### Question-1
query = """
WITH Players AS
(
Select \"Player 1\",\"ROUND\",tournament 
from 
allMatches 
UNION
Select \"Player 2\",\"ROUND\",tournament 
from 
allMatches 
)

SELECT 
tournament,\"Player 1\",Count(\"Player 1\") 
FROM Players
GROUP BY \"Player 1\",tournament
ORDER BY Count(\"Player 1\") DESC;
"""
df = pd.read_sql(query, engine)
df
#### Question-2
query_2_final = """
WITH Players AS
(
Select \"Player 1\",\"ROUND\",tournament,gender
from 
allMatches 
UNION
Select \"Player 2\",\"ROUND\",tournament,gender 
from 
allMatches 
)

SELECT 
tournament,\"Player 1\",Count(\"Player 1\"),gender 
FROM Players
GROUP BY gender,tournament,\"Player 1\"
HAVING Players.tournament in ('French Open','US Open','AUST Open') 
ORDER BY tournament,Count(\"Player 1\") DESC
;
"""
df = pd.read_sql(query_2_final, engine)
df
#### Question-3
query_3_final = """
WITH Players AS
(
Select \"Player 1\",\"ROUND\",tournament,\"FSP.1\" 
from 
allMatches 
UNION
Select \"Player 2\",\"ROUND\",tournament,\"FSP.2\" 
from 
allMatches 
)

SELECT 
\"Player 1\",\"FSP.1\" 
FROM Players
ORDER BY \"FSP.1\" DESC LIMIT 1;

"""
df = pd.read_sql(query_3_final, engine)
df

#### Question-4
query_4_final = """
WITH Players AS
(
Select 
\"Player 1\",\"ROUND\",tournament,\"FSP.1\",(\"DBF.1\")/NULLIF(\"DBF.1\"+\"UFE.1\",0) as error, 
CASE 
WHEN \"Result\" = 1 THEN 1 ELSE 0 END AS Win 
from 
allMatches 
UNION
Select \"Player 2\",\"ROUND\",tournament,\"FSP.2\",(\"DBF.2\")/NULLIF(\"DBF.2\"+\"UFE.2\",0) as error, 
CASE 
WHEN \"Result\" = 0 THEN 1 ELSE 0 END AS Win 
from 
allMatches 
),

WITH Players_win AS
(
Select \"Player 1\", SUM(Win)
From Players
GROUP BY \"Player 1\"
ORDER BY SUM(Win) DESC;
)

SELECT \"Player 1\"
FROM
(
SELECT \"Player 1\",RANK() OVER(PARTITION BY \"Player 1\" ORDER BY SUM(Win) DESC) AS the_rank
FROM Players
GROUP BY \"Player 1\"
)AS ordered
WHERE ordered.the_rank < 3;
"""
df = pd.read_sql(query_4_final, engine)
df