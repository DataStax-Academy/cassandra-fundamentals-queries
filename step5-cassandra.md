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
 <a href='command:katapod.loadPage?[{"step":"step4-cassandra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 5 of 11</span>
 <a href='command:katapod.loadPage?[{"step":"step6-cassandra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Create table "ratings_by_movie"</div>

Our second table will store information about ratings 
organized by movies, such that each partition will store all ratings for one 
particular movie. To define 
this table with *multi-row partitions*, we can use `title` and `year`, which uniquely identify a movie, 
as a *composite partition key* and `email`, which uniquely identifies a user,
as a *simple clustering key*. We will insert 4 rows into 3 partitions:


| title               | year | email            | rating |
|---------------------|------|------------------|--------|
| <span style="background-color:#F5B7B1">Alice in Wonderland</span> | <span style="background-color:#F5B7B1">2010</span> | jen@datastax.com |     10 |
| <span style="background-color:#F5B7B1">Alice in Wonderland</span> | <span style="background-color:#F5B7B1">2010</span> | joe@datastax.com |      9 |
| <span style="background-color:#AED6F1">Alice in Wonderland</span> | <span style="background-color:#AED6F1">1951</span> | jen@datastax.com |      8 |
| <span style="background-color:#ABEBC6">Edward Scissorhands</span> | <span style="background-color:#ABEBC6">1990</span> | joe@datastax.com |     10 |

<br/>

✅ Create the table:
```
CREATE TABLE IF NOT EXISTS ratings_by_movie (
  title TEXT,
  year INT,
  email TEXT,
  rating INT,
  PRIMARY KEY ((title, year), email)
);
```

✅ Insert the rows:
```
INSERT INTO ratings_by_movie (title, year, email, rating)  
VALUES ('Alice in Wonderland', 2010, 'jen@datastax.com', 10);
INSERT INTO ratings_by_movie (title, year, email, rating) 
VALUES ('Alice in Wonderland', 2010, 'joe@datastax.com', 9);
INSERT INTO ratings_by_movie (title, year, email, rating)   
VALUES ('Alice in Wonderland', 1951, 'jen@datastax.com', 8);
INSERT INTO ratings_by_movie (title, year, email, rating)   
VALUES ('Edward Scissorhands', 1990, 'joe@datastax.com', 10);
```

✅ Retrieve one row:
```
SELECT * FROM ratings_by_movie
WHERE title = 'Alice in Wonderland'
  AND year = 2010
  AND email = 'joe@datastax.com';
```

✅ Retrieve one partition:
```
SELECT * FROM ratings_by_movie
WHERE title = 'Alice in Wonderland'
  AND year = 2010;
```

✅ Retrieve all rows:
```
SELECT * FROM ratings_by_movie;
```

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step4-cassandra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step6-cassandra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

