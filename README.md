# W5D1 - SQL Intro

### To Do
- [ ] Introduction to RDBMS
- [ ] The Relational Data Model (Tables, Columns, Rows)
- [ ] SELECT Statements
- [ ] Filtering, ordering, limiting, etc.
- [ ] Joining tables
- [ ] Grouping records
- [ ] Aggregate functions

### RDBMS
- Relational DataBase Management System
- This is a database server
- `psql`
- Front End <=== HTTP/TCP ===> Back End <=== POSTGRES/TCP ===> RDBMS

### Database (relational database)
- Bunch of tables
- Table is a collection of rows and columns
- Rows === records
- Columns === attributes/fields/adjectives
- Table names are always plural
- Primary key/foreign keys
- All of the tables in a relational database are related to each other in some way

### SELECT Challenges

For the first 5 queries, we'll be using the `users` table.

1. List total number of users

```sql
SELECT COUNT(*) AS num_users
FROM users;
```

2. List users over the age of 18

```sql
SELECT *
FROM users
WHERE age > 18;
```

3. List users who are over the age of 18 and have the last name Barrows

```sql
SELECT *
FROM users
WHERE age > 18 
AND "last_name" = 'Barrows';
```

4. List users over the age of 18 who live in Canada sorted by age from oldest to youngest and then last name alphabetically

```sql
SELECT *
FROM users
WHERE age > 18
AND country = 'Canada'
ORDER BY age DESC, last_name ASC;
```

5. List users who live in Canada and whose accounts are overdue

```sql
SELECT *
FROM users
WHERE country = 'Canada'
AND payment_due_date < 'March 2, 2020';

-- we can also use the SQL function NOW to refer to the current time
SELECT *
FROM users
WHERE country = 'Canada'
AND payment_due_date < NOW();
```

For the rest of the queries, we'll be using the `albums` and `songs` tables.

6. List all albums along with their songs

```sql
SELECT * FROM albums
JOIN songs ON albums.id = songs.album_id;
```

7. List all albums along with how many songs each album has

```sql
SELECT album_name, COUNT(songs.id) AS num_songs
FROM albums
JOIN songs ON albums.id = songs.album_id
GROUP BY album_name;
```

8. Enhance previous query to only include albums that have more than 10 songs

```sql
SELECT album_name, COUNT(songs.id) AS num_songs
FROM albums
LEFT JOIN songs ON albums.id = songs.album_id
GROUP BY album_name
HAVING COUNT(songs.id) > 10;
```

9. List ALL albums in the database, along with their songs if any

```sql
SELECT album_name, songs.*
FROM albums
LEFT JOIN songs ON albums.id = songs.album_id;
```

10. List albums along with average song rating

```sql
SELECT album_name, AVG(songs.rating) AS avg_rating
FROM albums
JOIN songs ON albums.id = songs.album_id
GROUP BY album_name
ORDER BY avg_rating DESC;
```

11. List albums and songs with rating higher than album average

```sql
SELECT album_name, song_name, rating, (SELECT AVG(songs.rating) FROM songs WHERE albums.id = songs.album_id) as avg_rating
FROM albums
INNER JOIN songs ON albums.id = songs.album_id
WHERE rating > (SELECT AVG(songs.rating) FROM songs WHERE albums.id = songs.album_id);
```

### Useful Links
- [Top 10 Most Popular RDBMSs](https://www.c-sharpcorner.com/article/what-are-the-most-popular-relational-databases/)
- [Another Ranking of DBMSs](https://db-engines.com/en/ranking)
- [SELECT Queries Order of Execution](https://sqlbolt.com/lesson/select_queries_order_of_execution)
- [SQL Joins Visualizer](https://sql-joins.leopard.in.ua/)
