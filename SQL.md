# SQL

#### Section: Aggregate Functions

```mysql
#COUNT
SELECT COUNT(*) FROM books;
SELECT COUNT(DISTINCT author_fname) FROM books;
SELECT COUNT(DISTINCT author_lname, author_fname) from books;
SELECT COUNT(*) FROM books WHERE title LIKE '%the%';

#GROUP BY
SELECT title, author_lname FROM books GROUP BY author_lname;
SELECT author_lname, COUNT(*) FROM books GROUP BY author_fname, author_lname;
SELECT CONCAT('In ',released_year,' ', COUNT(*),' book(s) released.') AS 'year' FROM books group by released_year;

#MIN MAX
SELECT MIN(released_year) from books;
SELECT MAX(pages) FROM books;
SELECT * FROM books WHERE pages = (SELECT Min(Pages) FROM books);
SELECT * FROM books ORDER BY pages ASC LIMIT 1;

SELECT author_fname, author_lname, min(released_year) FROM books GROUP BY author_fname, author_lname;

#SUM
SELECT Sum(pages) FROM books;
SELECT author_fname, author_lname, SUM(pages) FROM books GROUP BY author_lname, author_fname;

#AVERAGE
SELECT AVG(released_year) FROM books;
SELECT released_year, AVG(stock_quantity) FROM books GROUP BY released_year;

SELECT COUNT(*) from books;
SELECT released_year, COUNT(*) FROM books GROUP BY released_year;
SELECT SUM(stock_quantity) FROM books;
SELECT author_fname, author_lname, AVG(released_year) FROM books GROUP BY author_fname, author_lname;
SELECT CONCAT(author_fname,' ', author_lname) AS 'fullname' FROM books ORDER BY pages DESC LIMIT 1;
SELECT CONCAT(author_fname,' ', author_lname) AS 'fullname' FROM books WHERE pages = (SELECT MAX(pages) from books);
SELECT released_year AS 'year', COUNT(*) AS'# books', AVG(pages) AS 'avg pages' FROM books GROUP BY released_year;
```

