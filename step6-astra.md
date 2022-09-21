<!-- TOP -->
<div class="top">
  <img src="https://datastax-academy.github.io/katapod-shared-assets/images/ds-academy-logo.svg" />
  <div class="scenario-title-section">
    <span class="scenario-title">Queries in Apache Cassandra®</span>
    <span class="scenario-subtitle">ℹ️ For technical support, please contact us via <a href="mailto:aleksandr.volochnev@datastax.com">email</a> or <a href="https://dtsx.io/aleks">LinkedIn</a>.</span>
  </div>
</div>

<!-- NAVIGATION -->
<div id="navigation-top" class="navigation-top">
 <a href='command:katapod.loadPage?[{"step":"step5-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 6 of 11</span>
 <a href='command:katapod.loadPage?[{"step":"step7-astra"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Create table "actors_by_movie"</div>

Our next table will store information about actors 
performing in movies, such that each partition will store all actors who 
performed in one particular movie. To define 
this table with *multi-row partitions*, we can use `title` and `year`, which uniquely identify a movie,
as a *composite partition key*, and `first_name` and `last_name`, which uniquely identify an actor, as a *composite clustering key*.
We will insert 3 rows into 2 partitions:

| title               | year | first_name       | last_name |
|---------------------|------|------------------|-----------|
| <span style="background-color:#F5B7B1">Alice in Wonderland</span> | <span style="background-color:#F5B7B1">2010</span> | Anne   | Hathaway |
| <span style="background-color:#F5B7B1">Alice in Wonderland</span> | <span style="background-color:#F5B7B1">2010</span> | Johnny | Depp     |
| <span style="background-color:#ABEBC6">Edward Scissorhands</span> | <span style="background-color:#ABEBC6">1990</span> | Johnny | Depp     |

This table is for you to create!

<br/>

✅ Create the table:
<details>
  <summary>Solution</summary>

```
CREATE TABLE IF NOT EXISTS actors_by_movie (
  title TEXT,
  year INT,
  first_name TEXT,
  last_name TEXT,
  PRIMARY KEY ((title, year), first_name, last_name)
);
```

</details>

<br/>

✅ Insert the rows:
<details>
  <summary>Solution</summary>

```
INSERT INTO actors_by_movie (title, year, first_name, last_name)  
VALUES ('Alice in Wonderland', 2010, 'Johnny', 'Depp');
INSERT INTO actors_by_movie (title, year, first_name, last_name) 
VALUES ('Alice in Wonderland', 2010, 'Anne', 'Hathaway');
INSERT INTO actors_by_movie (title, year, first_name, last_name)   
VALUES ('Edward Scissorhands', 1990, 'Johnny', 'Depp');
```

</details>

<br/>

✅ Retrieve one row:
<details>
  <summary>Solution</summary>

```
SELECT * FROM actors_by_movie
WHERE title = 'Alice in Wonderland'
  AND year = 2010
  AND first_name = 'Johnny'
  AND last_name = 'Depp';
```

</details>

<br/>

✅ Retrieve one partition:
<details>
  <summary>Solution</summary>

```
SELECT * FROM actors_by_movie
WHERE title = 'Alice in Wonderland'
  AND year = 2010;
```

</details>

<br/>

✅ Retrieve all rows:
<details>
  <summary>Solution</summary>

```
SELECT * FROM actors_by_movie;
```

</details>

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step5-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step7-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

