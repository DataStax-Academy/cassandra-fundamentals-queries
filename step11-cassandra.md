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
 <a href='command:katapod.loadPage?[{"step":"step10-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 11 of 12</span>
 <a href='command:katapod.loadPage?[{"step":"step12-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">What tables with multi-row partitions are good for?</div>

In Cassandra, tables are designed to support specific queries. While tables with 
single-row partitions are usually used to store and retrieve entities,  
tables with multi-row partitions are great for capturing relationships.
For example, we used tables `ratings_by_user` and `ratings_by_movie` to store and query relationships like 
*user rated many movies* and its reverse *movie was rated by many users*, respectively.
Tables with multi-row partitions also have a greater flexibility in terms of how data
can be retrieved, which includes equality queries, inequality or range queries, 
and ordering queries.

Finally, tables with multi-row partitions are also suitable for storing entities related based on some attribute values,
such as *movies with the same title* in the example below. Of course, whether you need such a table in your data model or not
depends on whether you need to support queries that retrieve movies based on titles.
 
```
CREATE TABLE IF NOT EXISTS movies_by_title (
  title TEXT,
  year INT,
  duration INT,
  avg_rating FLOAT,
  PRIMARY KEY ((title), year)
) WITH CLUSTERING ORDER BY (year DESC);

INSERT INTO movies_by_title (title, year, duration, avg_rating) 
VALUES ('Alice in Wonderland', 2010, 108, 6.00);
INSERT INTO movies_by_title (title, year, duration, avg_rating) 
VALUES ('Alice in Wonderland', 1951, 75, 7.08);

SELECT * FROM movies_by_title
WHERE title = 'Alice in Wonderland';
```

<!-- NAVIGATION -->
<div id="navigation-top" class="navigation-top">
 <a href='command:katapod.loadPage?[{"step":"step10-astra"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
 <a href='command:katapod.loadPage?[{"step":"step12-astra"}]'
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

