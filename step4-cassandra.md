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
 <a href='command:katapod.loadPage?[{"step":"step3-cassandra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 4 of 11</span>
 <a href='command:katapod.loadPage?[{"step":"step5-cassandra"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Create table "ratings_by_user"</div>

Our first table will store information about movie ratings 
organized by users, such that each partition will store all ratings left by one 
particular user. To define 
this table with *multi-row partitions*, we can use `email`, which uniquely identifies a user,
as a *simple partition key*, and `title` and `year`, which uniquely identify a movie, as a *composite clustering key*.
We will insert 4 rows into 2 partitions:

| email            | title               | year | rating |
|------------------|---------------------|------|--------|
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | Alice in Wonderland | 2010 |      9 |
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | Edward Scissorhands | 1990 |     10 |
| <span style="background-color:#AED6F1">jen@datastax.com</span> | Alice in Wonderland | 1951 |      8 |
| <span style="background-color:#AED6F1">jen@datastax.com</span> | Alice in Wonderland | 2010 |     10 |


<br/>

✅ Create the table:
```
DROP TABLE IF EXISTS ratings_by_user;
CREATE TABLE IF NOT EXISTS ratings_by_user (
  email TEXT,
  title TEXT,
  year INT,
  rating INT,
  PRIMARY KEY ((email), title, year)
);
```

✅ Insert the rows:
```
INSERT INTO ratings_by_user (email, title, year, rating) 
VALUES ('joe@datastax.com', 'Alice in Wonderland', 2010, 9);
INSERT INTO ratings_by_user (email, title, year, rating)  
VALUES ('joe@datastax.com', 'Edward Scissorhands', 1990, 10);
INSERT INTO ratings_by_user (email, title, year, rating) 
VALUES ('jen@datastax.com', 'Alice in Wonderland', 2010, 10);
INSERT INTO ratings_by_user (email, title, year, rating)  
VALUES ('jen@datastax.com', 'Alice in Wonderland', 1951, 8);
```

✅ Retrieve one row:
```
SELECT * FROM ratings_by_user
WHERE email = 'joe@datastax.com'
  AND title = 'Alice in Wonderland'
  AND year = 2010;
```

✅ Retrieve one partition:
```
SELECT * FROM ratings_by_user
WHERE email = 'joe@datastax.com';
```

✅ Retrieve all rows:
```
SELECT * FROM ratings_by_user;
```

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step3-cassandra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step5-cassandra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

