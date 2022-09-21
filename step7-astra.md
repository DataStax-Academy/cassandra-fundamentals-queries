<!-- TOP -->
<div class="top">
  <img src="https://datastax-academy.github.io/katapod-shared-assets/images/ds-academy-logo.svg" />
  <div class="scenario-title-section">
    <span class="scenario-title">Tables with Multi-Row Partitions in Apache Cassandra®</span>
    <span class="scenario-subtitle">ℹ️ For technical support, please contact us via <a href="mailto:aleksandr.volochnev@datastax.com">email</a> or <a href="https://dtsx.io/aleks">LinkedIn</a>.</span>
  </div>
</div>

<!-- NAVIGATION -->
<div id="navigation-top" class="navigation-top">
 <a href='command:katapod.loadPage?[{"step":"step6-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 7 of 12</span>
 <a href='command:katapod.loadPage?[{"step":"step8-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Create table "movies_by_actor"</div>

Next, we can design another table to store the inverse relationship of movies by actors, 
such that each partition will store all movies where a given actor performed. To define 
this table with *multi-row partitions*, we can use `first_name` and `last_name`, which uniquely identify an actor,
as a *composite partition key*, and `title` and `year`, which uniquely identify a movie, as a *composite clustering key*.
We will insert 3 rows into 2 partitions:

| first_name | last_name | title               | year |
|------------|-----------|---------------------|------|
|     <span style="background-color:#F5B7B1">Johnny</span> |  <span style="background-color:#F5B7B1">    Depp</span> | Alice in Wonderland | 2010 |
|     <span style="background-color:#F5B7B1">Johnny</span> |  <span style="background-color:#F5B7B1">    Depp</span> | Edward Scissorhands | 1990 |
|     <span style="background-color:#ABEBC6">  Anne</span> |  <span style="background-color:#ABEBC6">Hathaway</span> | Alice in Wonderland | 2010 |

This table is for you to create!

<br/>

✅ Create the table:
<details>
  <summary>Solution</summary>

```
CREATE TABLE IF NOT EXISTS movies_by_actor (
  first_name TEXT,
  last_name TEXT,
  title TEXT,
  year INT,  
  PRIMARY KEY ((first_name, last_name), title, year)
);
```

</details>

<br/>

✅ Insert the rows:
<details>
  <summary>Solution</summary>

```
INSERT INTO movies_by_actor (first_name, last_name, title, year)  
VALUES ('Johnny', 'Depp', 'Alice in Wonderland', 2010);
INSERT INTO movies_by_actor (first_name, last_name, title, year)   
VALUES ('Johnny', 'Depp', 'Edward Scissorhands', 1990);
INSERT INTO movies_by_actor (first_name, last_name, title, year)
VALUES ('Anne', 'Hathaway', 'Alice in Wonderland', 2010);
```

</details>

<br/>

✅ Retrieve one row:
<details>
  <summary>Solution</summary>

```
SELECT * FROM movies_by_actor
WHERE first_name = 'Johnny'
  AND last_name = 'Depp'
  AND title = 'Alice in Wonderland'
  AND year = 2010;
```

</details>

<br/>

✅ Retrieve one partition:
<details>
  <summary>Solution</summary>

```
SELECT * FROM movies_by_actor
WHERE first_name = 'Johnny'
  AND last_name = 'Depp';
```

</details>

<br/>

✅ Retrieve all rows:
<details>
  <summary>Solution</summary>

```
SELECT * FROM movies_by_actor;
```

</details>

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step6-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step8-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

