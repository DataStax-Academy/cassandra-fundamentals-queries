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
 <a href='command:katapod.loadPage?[{"step":"step9-cassandra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 10 of 11</span>
 <a href='command:katapod.loadPage?[{"step":"step11-cassandra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Static columns</div>

Some non-key columns in a table with *multi-row partitions* can be declared 
as *static columns*. A *static column* is shared by all rows belonging to 
the same partition. Therefore, if you know that a partition key functionally determines 
a non-key column in a table with multi-row partitions, then the non-key column will always have the 
same value for any row in a partition and should be declared *static* to optimize 
the storage and simplify column updates.

A static column is defined within the `CREATE TABLE` statement using the `STATIC` keyword, 
which we purposefully omitted in our previous discussions for simplicity.

Let's re-design table `ratings_by_user` to store not only movie ratings organized by users,
but also user names and ages as shown in the example below. Notice that `email`, `name`, and `age`
are all attributes or descriptors of users. Moreover, `email` unique identifies both a user and a partition in
the table, such that any row in the same partition will have the same `name` and `age`.

| email            | title               | year | rating | name | age |
|------------------|---------------------|------|--------|------|-----|
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | Alice in Wonderland | 2010 |      9 | <span style="background-color:#FDEDEC">Joe</span> | <span style="background-color:#FDEDEC">25</span> |
| <span style="background-color:#F5B7B1">joe@datastax.com</span> | Edward Scissorhands | 1990 |     10 | <span style="background-color:#FDEDEC">Joe</span> | <span style="background-color:#FDEDEC">25</span> |
| <span style="background-color:#AED6F1">jen@datastax.com</span> | Alice in Wonderland | 1951 |      8 | <span style="background-color:#EBF5FB">Jen</span> | <span style="background-color:#EBF5FB">27</span> |
| <span style="background-color:#AED6F1">jen@datastax.com</span> | Alice in Wonderland | 2010 |     10 | <span style="background-color:#EBF5FB">Jen</span> | <span style="background-color:#EBF5FB">27</span> |

<br/>

✅ Drop the existing table:
```
DROP TABLE IF EXISTS ratings_by_user;
```

<br/>

✅ Create the new table:
```
CREATE TABLE ratings_by_user (
  email TEXT,
  title TEXT,
  year INT,
  rating INT,
  name TEXT STATIC,
  age INT STATIC,
  PRIMARY KEY ((email), title, year)
);
```

<br/>

✅ Insert the rows:
```
INSERT INTO ratings_by_user (email, title, year, rating, name, age) 
VALUES ('joe@datastax.com', 'Alice in Wonderland', 2010, 9, 'Joe', 25);
INSERT INTO ratings_by_user (email, title, year, rating, name, age) 
VALUES ('joe@datastax.com', 'Edward Scissorhands', 1990, 10, 'Joe', 25);
INSERT INTO ratings_by_user (email, title, year, rating, name, age)
VALUES ('jen@datastax.com', 'Alice in Wonderland', 2010, 10, 'Jen', 27);
INSERT INTO ratings_by_user (email, title, year, rating, name, age)
VALUES ('jen@datastax.com', 'Alice in Wonderland', 1951, 8, 'Jen', 27);
```

<br/>

✅ Verify the result:
```
SELECT * FROM ratings_by_user;
```

<br/>

✅ Update static columns:
```
UPDATE ratings_by_user SET name = 'Joseph' 
WHERE email = 'joe@datastax.com';
UPDATE ratings_by_user SET age = 28 
WHERE email = 'jen@datastax.com';
```

<br/>

✅ Verify the result:
```
SELECT * FROM ratings_by_user;
```

<!-- NAVIGATION -->
<div id="navigation-top" class="navigation-top">
 <a href='command:katapod.loadPage?[{"step":"step9-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step11-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

