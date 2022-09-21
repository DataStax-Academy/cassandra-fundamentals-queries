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
 <a href='command:katapod.loadPage?[{"step":"step8-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 9 of 11</span>
 <a href='command:katapod.loadPage?[{"step":"step10-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Create table "movies_by_user"</div>

This next table will store information about movies that users watched, 
such that each partition will store all movies for one particular user 
ordered by the dates of when the user watched them. Here are 6 rows to be inserted into 2 partitions:

| email            | watched_on | title               | year |
|------------------|------------|---------------------|------|
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | 2020-04-28 | Edward Scissorhands | 1990 |
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | 2020-03-08 | Alice in Wonderland | 2010 | 
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | 2020-02-13 |         Toy Story 3 | 2010 |
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | 2020-01-22 |       Despicable Me | 2010 |
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | 2019-12-30 | Alice in Wonderland | 1951 |
| <span style="background-color:#ABEBC6">jen@datastax.com</span> | 2011-10-01 | Alice in Wonderland | 2010 |


This table is for you to create!

<br/>

✅ Create the table:
<details>
  <summary>Solution</summary>

```
CREATE TABLE IF NOT EXISTS movies_by_user (
  email TEXT,
  title TEXT,
  year INT,
  watched_on DATE,
  PRIMARY KEY ((email), watched_on, title, year)
) WITH CLUSTERING ORDER BY (watched_on DESC, title ASC, year ASC);
```

</details>

<br/>

✅ Insert the rows:
<details>
  <summary>Solution</summary>

```
INSERT INTO movies_by_user (email, watched_on, title, year) 
VALUES ('joe@datastax.com', '2020-01-22', 'Despicable Me', 2010);
INSERT INTO movies_by_user (email, watched_on, title, year) 
VALUES ('joe@datastax.com', '2020-02-13', 'Toy Story 3', 2010);
INSERT INTO movies_by_user (email, watched_on, title, year) 
VALUES ('joe@datastax.com', '2019-12-30', 'Alice in Wonderland', 1951);
INSERT INTO movies_by_user (email, watched_on, title, year) 
VALUES ('joe@datastax.com', '2020-03-08', 'Alice in Wonderland', 2010);
INSERT INTO movies_by_user (email, watched_on, title, year) 
VALUES ('joe@datastax.com', '2020-04-28', 'Edward Scissorhands', 1990);
INSERT INTO movies_by_user (email, watched_on, title, year) 
VALUES ('jen@datastax.com', '2011-10-01', 'Alice in Wonderland', 2010);
```

</details>

<br/>

✅ Retrieve one partition:
<details>
  <summary>Solution</summary>

```
SELECT * FROM movies_by_user
WHERE email = 'joe@datastax.com';
```

</details>

<br/>

✅ Retrieve one partition using the reverse row ordering:
<details>
  <summary>Solution</summary>

```
SELECT * FROM movies_by_user
WHERE email = 'joe@datastax.com'
ORDER BY watched_on ASC;
```

</details>

<br/>

✅ Retrieve a subset of rows from one partition:
<details>
  <summary>Solution</summary>

```
SELECT * FROM movies_by_user
WHERE email = 'joe@datastax.com'
  AND watched_on > '2020-01-01';
```

</details>

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step8-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step10-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

