##### Task 0: Data Import
- Importing comma-separated value (CSV) file taxonomy_iw.csv.gz which represented Wikipedia main topic classification categories. Each line is a single record with two fields indicating category-subcategory relationship such as "1880s_films", "1889_films" indicates that there is a "1880s_films" category which has "1889_films" subcategory.

- _CSV file into a database_:

```SQL
cd Desktop
cd project
ls
scp * s414151@labagh.pl:

```

```SQL
-- For PostgreSQL
\copy taxonomy FROM 'taxonomy_iw.csv'  CSV ;
```

##### **Task 1: Find children of a given node**
- Creating table for store data 
```SQL
CREATE TABLE taxonomy (
  category text,
  subcategory text
);
```

```SQL
CREATE TABLE taxonomy (
  category VARCHAR,
  subcategory VARCHAR
);
```

```SQL
SELECT category, subcategory
FROM taxonomy
WHERE category = '1880s_films';
```

- **_Recursive_** query for finding children of given node
```SQL
WITH RECURSIVE children AS (
  SELECT subcategory
  FROM taxonomy
  WHERE category = '1880s_films' -- changeable
  UNION
  SELECT t.subcategory
  FROM taxonomy t
  INNER JOIN children c ON t.category = c.subcategory
)
SELECT * FROM children;
```


##### **Task 2: Count all children of a given node**
- As in the 1st task, same process must be apply for creating table to store data and import data from the CSV file.

```SQL
WITH RECURSIVE children AS (
  SELECT subcategory
  FROM taxonomy
  WHERE category = '1880s_films' -- replace 
  UNION
  SELECT t.subcategory
  FROM taxonomy t
  INNER JOIN children c ON t.category = c.subcategory
)
SELECT COUNT(*) AS child_count FROM children;
```

##### **Task 3: Find all grandchildren of a given node**
```SQL
SELECT * FROM grandchildren;
```

```SQL
WITH RECURSIVE grandchildren AS (
  SELECT subcategory
  FROM taxonomy
  WHERE category = '1880s_films' 

  UNION

  SELECT t.subcategory
  FROM taxonomy t
  INNER JOIN grandchildren gc ON t.category = gc.subcategory
)
SELECT *
FROM grandchildren;
```

##### **Task 4: Find all parents of a given node**
```SQL
SELECT * FROM parents;
```
```SQL
WITH RECURSIVE parents AS (
  SELECT category
  FROM taxonomy
  WHERE subcategory = '1880s_films' -- replace 
  UNION
  SELECT t.category
  FROM taxonomy t
  INNER JOIN parents p ON t.subcategory = p.category
)
```
##### **Task 5: Counts all parents of a given node**
- working
```SQL
SELECT COUNT(*) AS parent_count FROM parents;
```
```SQL
WITH RECURSIVE parents AS (
  SELECT category
  FROM taxonomy
  WHERE subcategory = '1880s_films' -- replace
  UNION
  SELECT t.category
  FROM taxonomy t
  INNER JOIN parents p ON t.subcategory = p.category
)
SELECT COUNT(*) AS parent_count FROM parents;
```
##### **Task 6: Finds all grandparents of a given node**
- working
```SQL
SELECT * FROM grandparents;
```
```SQL
WITH RECURSIVE grandparents AS (
  SELECT category
  FROM taxonomy
  WHERE subcategory = '1880s_films' -- replace 
  UNION
  SELECT t.category
  FROM taxonomy t
  INNER JOIN grandparents g ON t.subcategory = g.category
)
SELECT * FROM grandparents;
```
##### **Task 7: Counts how many uniquely named nodes there are**
- working
```SQL
SELECT COUNT( node) AS node_count
FROM (
  SELECT category AS node FROM taxonomy
  UNION
  SELECT subcategory AS node FROM taxonomy
) AS combined_nodes;
```
##### **Task 8: Finds root node, one which is not a subcategory of any other node**
- working
```SQL
SELECT category
FROM taxonomy
WHERE category NOT IN (SELECT subcategory FROM taxonomy WHERE subcategory IS NOT NULL);
```
##### **Task 9: Finds nodes with the most children, there could be more than one**
- working
```SQL
WITH child_counts AS (
  SELECT category, COUNT(subcategory) AS child_count
  FROM taxonomy
  GROUP BY category
)
SELECT category, child_count
FROM child_counts
WHERE child_count = (
  SELECT MAX(child_count)
  FROM child_counts
);
```
##### **Task 10: Finds nodes with the least children, there could be more than one**
- **Query** to find the nodes and child counts 
```SQL
SELECT category, COUNT(subcategory) AS child_count
FROM taxonomy
GROUP BY category
```
- Finding **minimum** child count
```SQL
SELECT MIN(child_count) AS min_child_count
FROM (
  SELECT COUNT(subcategory) AS child_count
  FROM taxonomy
  GROUP BY category
) AS child_counts
```
- Retrieve the nodes regarding to minimum child count
- This query will select the categories that have a child count equal to the minimum child count determined in the previous step
```SQL
WITH category_counts AS (
  SELECT category, COUNT(subcategory) AS num_children
  FROM taxonomy
  GROUP BY category
),
min_child_count AS (
  SELECT MIN(num_children) AS min_count
  FROM category_counts
)
SELECT category
FROM category_counts
WHERE num_children = (SELECT min_count FROM min_child_count);
```
-- "category_counts" is used to calculate the number of children (subcategories) for each category by grouping and counting the subcategories
##### **Task 11: Renames given node**
```SQL
UPDATE taxonomy
SET category = 'new_name'
WHERE category = 'old_name';
```
