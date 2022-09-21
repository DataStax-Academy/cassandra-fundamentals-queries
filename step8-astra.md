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
 <a href='command:katapod.loadPage?[{"step":"step7-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 8 of 11</span>
 <a href='command:katapod.loadPage?[{"step":"step9-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Clustering order</div>

Besides serving as a unique row identifier within a partition, a clustering key also defines
how rows are ordered within the same partition. In case of a composite clustering key,
rows are first ordered by the first clustering key column, then the second one and so forth. It 
should now be evident that `PRIMARY KEY ((email), title, year)` and `PRIMARY KEY ((email), year, title)`
are not the same. The clustering order affects both storage and retrieval, including efficient range queries on clustering key 
columns. Rows are always retrieved using the clustering order or its reverse.
While the default order for each column is ascendant or `ASC`, it can be customized on a per column basis
using the `CLUSTERING ORDER BY` clause. Check out the following example.

✅ Drop the existing table:
```
DROP TABLE IF EXISTS ratings_by_user;
```

✅ Create the new table:
```
CREATE TABLE IF NOT EXISTS ratings_by_user (
  email TEXT,
  year INT,
  title TEXT,
  rating INT,
  PRIMARY KEY ((email), year, title)
) WITH CLUSTERING ORDER BY (year DESC, title ASC);
```

✅ Insert the rows:
```
INSERT INTO ratings_by_user (email, year, title, rating) 
VALUES ('joe@datastax.com', 2010, 'Despicable Me', 5);
INSERT INTO ratings_by_user (email, year, title, rating) 
VALUES ('joe@datastax.com', 2010, 'Toy Story 3', 10);
INSERT INTO ratings_by_user (email, year, title, rating) 
VALUES ('joe@datastax.com', 1951, 'Alice in Wonderland', 7);
INSERT INTO ratings_by_user (email, year, title, rating) 
VALUES ('joe@datastax.com', 2010, 'Alice in Wonderland', 9);
INSERT INTO ratings_by_user (email, year, title, rating) 
VALUES ('joe@datastax.com', 1990, 'Edward Scissorhands', 10);
INSERT INTO ratings_by_user (email, year, title, rating) 
VALUES ('jen@datastax.com', 2010, 'Alice in Wonderland', 10);
```

✅ Retrieve one partition:
```
SELECT * FROM ratings_by_user
WHERE email = 'joe@datastax.com';
```

✅ Retrieve one partition using the reverse row ordering:
```
SELECT * FROM ratings_by_user
WHERE email = 'joe@datastax.com'
ORDER BY year ASC, title DESC;
```

✅ Retrieve a subset of rows from one partition:
```
SELECT * FROM ratings_by_user
WHERE email = 'joe@datastax.com'
  AND year < 2000;
```

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step7-astra"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step9-astra"}]'
    class="btn btn-dark navigation-bottom-right">Next ➡️
  </a>
</div>

