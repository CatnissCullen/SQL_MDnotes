# SQL Grammar

****



## ==DDL==

***DEFINE A RELATION AS A TABLE WITH:***

-   ***`schema` (attributes' names)***
-   ***`domains`***
-   ***`integrity constraints`***
-   ***`indices`***
-   ***`security & authorization`***
-   ***`storage structure`*** 

***USING:***

-   **`CREATE`**
-   **`ALTER`**
-   **`DROP`**
-   **`TRUNCATE`**
-   **`COMMENT`**
-   **`RENAME`**

### ==*Schema/ Domains/ Integrity Constraints*==

***`Integrity Constraints`:***

-   **`PRIMARY KEY`**
-   **`FOREIGN KEY`**
-   **`NOT NULL`**
-   **`CHECK`**
-    **`UNIQUE`**
-   (naming) **`CONSTRAINT`** 

>   #### `CREATE TABLE`
>
>   ```sql
>   CREATE TABLE <relation_name> (
>       attribute_1 <domain_1>,
>       attribute_2 <domain_2> NOT NULL,
>       attribute_3 <domain_3> CHECK(attribute_3>0),  # check-clause usage 1 (inline)
>       attribute_4 <domain_4>
>       ...
>       attribute_n <domain_n>,
>       PRIMARY KEY(attribute_1),
>       FOREIGN KEY(attribute_2) REFERENCES <another_relation_name> ON DELETE SET NULL,
>       CONSTRAINT <constraint_name> CHECK(attribute_4 IN (`v1`,`v2`,`v3`))  # check-clause usage 2 (specifying a enumerated domain)
>       CHECK(attribute_n>1)  # check-clause usage 3 (outline)
>   )
>       
>   ```
>
>   #### `DROP TABLE`
>
>   ***the whole table is gone!!!!!!!!***
>
>   ```SQL
>   DROP TABLE <relation_name>
>   ```
>
>   #### `ALTER TABLE`
>
>   ***alter attributes & constraints***
>
>   ``` SQL
>   ALTER TABLE <relation_name> ADD|ALTER <attribute_name> <domain>
>   ALTER TABLE <relation_name> DROP <attribute_name>
>   ALTER TABLE <relation_name> DROP CONSTRAINT <constraint_name>
>   ```
>
>   #### `AS`
>
>   ***used to RENAME relations or attributes***
>
>   ```sql
>   SELECT r1.attr_1, r2.attr_2 FROM relation_x AS r1, relation_x AS r2 
>   WHERE r1.attr_1=r2.attr_2  # renaming relations
>   SELECT attr_i xxx_a FROM <relation_name>  # renaming attributes; AS can always be ommited!! 
>   ```

### ==*Indices*==

***Using the Search-key***

***TYPES OF INDEX:***

-   Primary Index *(also called  Clustering Index; the search-key is usually but not necessary the primary key)*
-   Secondary Index *(also called Non-clustering Index)*
-   Unique Index

>   #### `CREATE INDEX`
>
>   ```SQL
>   CREATE UNIQUE|CLUSTER|<none> INDEX <index_name> ON <relation_name>(attr_i, attr_j, attr_k) ASC|DESC
>   ```
>
>   #### `DROP INDEX`
>
>   ```SQL
>   DROP INDEX <index_name>
>   ```





## ==DML==

***MANIPULATE DATA IN TABLE USING:***

-   **`SELECT`**
-   **`INSERT`**
-   **`UPDATE`**
-   **`DELETE`**
-   **`MERGE`**
-   **`CALL`**
-   **`EXPLAIN PLAN`**
-   **`LOCK TABLE`**

>   #### `SELECT`
>
>   ##### ***collaborate with `WHERE` & `FROM` & `ORDER BY` & `GROUP BY`***
>
>   ***execute sequence: `FROM` -> `WHERE` -> `GROUP BY` -> `SELECT` -> `ORDER BY`***
>
>   ```SQL
>   SELECT DISTINCT|ALL|<none> (attr_i, attr_j, attr_k) FROM <relation_name>  # default = ALL
>   SELECT * FROM <relation_name>
>   SELECT attr_i+10 FROM <relation_name>
>   SELECT attr_i FROM <relation_name> WHERE attr_j>5 AND|OR attr_k=8
>   SELECT attr_i FROM <relation_name> WHERE NOT attr_j<4
>   SELECT attr_i FROM <relation_name> WHERE attr_j BETWEEN 2 AND 7
>   SELECT attr_i FROM <relation_name> WHERE attr_j LIKE '%abc%'  # % means any substring
>   SELECT attr_i FROM <relation_name> WHERE attr_j LIKE '_abc_'  # _ means any character
>   SELECT attr_i FROM <relation_1>, <relation_2>  # the CP of the 2 relations
>   SELECT attr_i FROM <relation_name> ORDER BY attr_j ASC|DESC|<none>  # default = ASC
>   SELECT attr_i FROM <relation_name> GROUP BY (attr_i, attr_j, attr_k)  # after GROUP BY aggregated function will act on group instead of the whole tuples; SELECTed attributes must appear in GROUP BY!!!!!
>   SELECT attr_i FROM <relation_name> GROUP BY (attr_i, attr_j, attr_k) HAVING attr_j<15  # HAVING is usually used after GROUP BY
>   ```
>
>   ##### ***collaborate with Aggregated Functions***
>
>   ***return a single value***
>
>   ```SQL
>   # ignore NULL
>   SELECT AVG(DISTINCT|ALL|<none> <attribute_name>) FROM <relation_name>  # default = ALL
>   SELECT MIN(DISTINCT|ALL|<none> <attribute_name>) FROM <relation_name>
>   SELECT MAX(DISTINCT|ALL|<none> <attribute_name>) FROM <relation_name>
>   SELECT SUM(DISTINCT|ALL|<none> <attribute_name>) FROM <relation_name>
>   # notice NULL
>   SELECT COUNT(DISTINCT|ALL|<none> <attribute_name>) FROM <relation_name>
>   SELECT COUNT(DISTINCT|ALL|<none> *) FROM <relation_name>  # count the number of tuples in a table
>   ```
>
>   #### `INSERT INTO`
>
>   ***insert data (tuples) to table***
>
>   ```SQL
>   INSERT INTO <relation_name> VALUES <tuple_of_values_of_all_attributes>
>   INSERT INTO <relation_name>(attr_i, attr_j, attr_k) VALUES(v_i, v_j, v_k)  # other attributes will have default values 
>   ```
>
>   #### `UNION, INTERSECT, EXCEPT`
>
>   ***auto DISTINCT if not using ALL***
>
>   ```sqL
>   (<select_clause_1>) UNION|INTERSECT|EXCEPT ALL|<none> (<select_clause_2>)
>   ```
