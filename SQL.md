

# MySQL

#### Section: String Functions

```mysql
SELECT UPPER(REVERSE('Why does my cat look at me with such hatred?'));
SELECT REPLACE(title,' ','->') AS 'title' from books;
SELECT author_lname AS 'forwards', REVERSE(author_lname) AS 'backward' from books;
SELECT CONCAT(UPPER(author_fname),' ',UPPER(author_lname)) AS 'full name in caps' from books;
SELECT CONCAT(title,' was released in ',released_year) AS 'blurb' from books;
SELECT title, CHAR_LENGTH(title) AS 'character count' from books;
SELECT CONCAT(SUBSTR(title, 1, 10),'...') AS 'short title', CONCAT(author_lname,',',author_fname) AS 'author', CONCAT(stock_quantity,' in stock') AS 'quantity' from books;
```



#### Section:  Aggregate Functions

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

##### Data Types

```mysql
INSERT INTO people(name, birthdate, birthtime, birthdt)
VALUES
(
	'Apple Pie', CURDATE(), CURTIME(), NOW()
);

SELECT name, DAY(birthdate) FROM people;
SELECT name, DAYNAME(birthdate) FROM people;
SELECT name, DAYOFWEEK(birthdate) FROM people;
SELECT name, DAYOFYEAR(birthdate) FROM people;

MONTHNAME()
YEAR()
SELECT CONCAT(MONTHNAME(birthdate),' ',DAY(birthdate),' ',YEAR(birthdate)) from people;
SELECT DATE_FORMAT(birthdt, '%W %M %Y') from people;
SELECT DATE_FORMAT(birthdt, '%m\%d\%Y') from people;

#DATE MATH
SELECT DATEDIFF(NOW(), birthdate) from people;
SELECT birthdt, DATE_ADD(birthdt, INTERVAL 1 MONTH) FROM people;
SELECT birthdt, birthdt + INTERVAL 5 MONTH FROM people;

#TIMESTAMPS
#TIMESTAMP has a range of '1970-01-01 00:00:01' to '2038-01-19 03:14:07'
CREATE TABLE comments(
	content VARCHAR(100),
	created_at TIMESTAMP DEFAULT NOW()
);
INSERT INTO comments(content)
VALUES ('HAHAHA');

CREATE TABLE comments2(
	content VARCHAR(100),
	changed_at TIMESTAMP DEFAULT NOW() ON UPDATE CURRENT_TIMESTAMP
);
UPDATE comments2 SET content = "I like chips." WHERE content = 'lol';

#USE CHAR when the digits are fixed
CREATE TABLE inventory(
	item_name VARCHAR(100),
	price DECIMAL(8,2),
	quantity INT
);
#Difference between DATETIME and TIMESTAMP:
#TIMESTAMP has a smaller range. 
#TIMESTAMP also takes up less space. 
#TIMESTAMP is used for things like meta-data about when something is created
#or updated.

SELECT DAYOFWEEK(CURDATE());
SELECT DAYNAME(CURDATE());
SELECT DATE_FORMAT(NOW(),'%m/%d/%Y');
#https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format
SELECT DATE_FORMAT(NOW(),'%M %D at %k:%i');
CREATE TABLE tweets(
	content text,
	username VARCHAR(50),
	created_at TIMESTAMP DEFAULT NOW()
);
```

#### Logical Operator

- !=
- NOT LIKE
- GREATER THAN LESSER THAN
- IN / NOT IN
- CASE STATEMENTS

```mysql
SELECT title FROM books WHERE released_year != 2017;
SELECT title FROM books WHERE title NOT LIKE 'W%';

SELECT title, released_year FROM books 
WHERE released_year > 2000 ORDER BY released_year;

'a' > 'b'
-- false
 
'A' > 'a'
-- false
 
'A' >=  'a'
-- true

#Logical AND &&
SELECT  
    title, 
    author_lname, 
    released_year FROM books
WHERE author_lname='Eggers' 
    AND released_year > 2010;
	
#Logical OR
SELECT * FROM books WHERE author_lname = 'Eggers' || released_year > 2010;

#BETWEEN
SELECT title, released_year FROM books WHERE released_year >=2004 && released_year <= 2015;
SELECT title, released_year FROM books WHERE released_year BETWEEN 2004 AND 2015;
SELECT title, released_year FROM books WHERE released_year NOT BETWEEN 2004 AND 2015;

SELECT CAST('2017-05-02' AS DATETIME);
SELECT name, birthdt BETWEEN FROM people
WHERE birthdt BETWEEN CAST('1980-01-01' AS DATETIME) 
				AND CAST('2010-01-01' AS DATETIME);
				
#IN / NOT IN
SELECT title, author_lname FROM books WHERE author_lname IN('Carver', 'Lahiri', 'Smith');
SELECT title, authot_lname FROm books WHERE 
released_year >= 2000 AND
released_year NOT IN
(2000, 2002, 2004, 2006, 2008, 2010);

SELECT title, released_year FROM books
WHERE released_year >= 2000 AND
released_year % 2 != 0;

#CASE STATEMENTS
SELECT title, released_year,
		CASE
		  WHEN released_year >= 2000 THEN 'Modern Lit'
		  ELSE '20th Century Lit'
		END AS GENRE
FROM books;

SELECT title, stock_quantity,
	CASE
		WHEN stock_quantity BETWEEN 0 AND 50 THEN '*'
		WHEN stock_quantity BETWEEN 51 AND 100 THEN '**'
		ELSE '***'
	END AS STOCK
FROM books;

SELECT title, stock_quantity,
	CASE
		WHEN stock_quantity <= 50 THEN '*'
		WHEN stock_quantity <= 100 THEN '**'
		ELSE '***'
	END AS STOCK
FROM books;

#0 1 1
SELECT * FROM books WHERE released_year < 1980;
SELECT * FROM books WHERE author_lname = 'Eggers' || author_lname = 'Chabon';
SELECT * FROM books WHERE author_lname IN ('Eggers', 'Chabon');
SELECT * FROM books WHERE author_lname = 'Lahiri' && released_year >2000;
SELECT * FROM books WHERE pages BETWEEN 100 AND 200;

SELECT * FROM books WHERE author_lname LIKE 'C%' || author_lname LIKE 'S%';
SELECT title, author_lname FROM books 
WHERE SUBSTR(author_lname,1,1) IN ('C', 'S');

SELECT title, author_lname,
	CASE 
		WHEN title LIKE '%stories%' THEN 'Short Stories'
		WHEN title ='Just Kids' || title LIKE '%A Heartbreaking Work%' THEN 'Memoir'
		ELSE 'Novel'
		END AS 'TYPE'
FROM books;

SELECT title, author_lname,
	CASE 
		WHEN (COUNT(*)) = 1 THEN '1 book'
		ELSE CONCAT(COUNT(*),' books')
		END AS 'COUNT'
FROM books GROUP BY author_fname, author_lname ORDER BY author_lname;
```

##### One to Many Relationship

```mysql
CREATE TABLE customers(
	id INT AUTO_INCREMENT PRIMARY KEY,
	first_name VARCHAR(100),
	last_name VARCHAR(100),
	email VARCHAR(100)
);

CREATE TABLE orders(
	id INT AUTO_INCREMENT PRIMARY KEY,
	order_date DATE,
	amount DECIMAL(8,2),
	customer_id INT,
	FOREIGN KEY(customer_id) REFERENCES customers(id)
);

INSERT INTO customers (first_name, last_name, email) 
VALUES ('Boy', 'George', 'george@gmail.com'),
       ('George', 'Michael', 'gm@gmail.com'),
       ('David', 'Bowie', 'david@gmail.com'),
       ('Blue', 'Steele', 'blue@gmail.com'),
       ('Bette', 'Davis', 'bette@aol.com');
       
INSERT INTO orders (order_date, amount, customer_id)
VALUES ('2016/02/10', 99.99, 1),
       ('2017/11/11', 35.50, 1),
       ('2014/12/12', 800.67, 2),
       ('2015/01/03', 12.50, 2),
       ('1999/04/11', 450.25, 5);
	   
SELECT * FROM orders WHERE customer_id = 1;
SELECT * FROM orders WHERE customer_id = 
(
	SELECT id FROM customers
	WHERE last_name = 'George'
);
#CROSS JOIN - USELESS
SELECT * FROM customers, orders;

#IMPLICIT INNER JOIN
SELECT * FROM customers, orders WHERE customers.id = orders.customer_id;
SELECT first_name, last_name, order_date, amount
FROM customers, orders 
WHERE customers.id = orders.customer_id;
-- EXPLICIT INNER JOIN *SUGGEST*
SELECT first_name, last_name, order_date, amount 
FROM customers
INNER JOIN orders
	ON customers.id = orders.customer_id;
-- order doesn't matter here
SELECT first_name, last_name, order_date, amount 
FROM orders
JOIN customers
	ON customers.id = orders.customer_id;
	
SELECT 
	first_name,
	last_name, 
	SUM(amount) AS total_spent
FROM customers
JOIN orders
	ON customers.id = orders.customer_id
GROUP BY orders.customer_id
ORDER BY total_spent DESC;

-- LEFT JOIN
SELECT first_name, last_name, order_date, amount 
FROM customers
LEFT JOIN orders
	ON customers.id = orders.customer_id;

SELECT 
	first_name, 
	last_name, 
	IFNULL(SUM(amount), 0) AS total_spent
FROM customers
LEFT JOIN orders
	ON customers.id = orders.customer_id
GROUP BY customers.id
ORDER BY total_spent;

-- RIGHT JOIN same as INNER JOIN here
SELECT first_name, last_name, order_date, amount 
FROM customers
RIGHT JOIN orders
	ON customers.id = orders.customer_id;

-- DROP orders FIRST then customers
DROP TABLE orders, customers;

-- ON Delete CASCADE
CREATE TABLE orders(
	id INT AUTO_INCREMENT PRIMARY KEY,
	order_date DATE,
	amount DECIMAL(8,2),
	customer_id INT,
	FOREIGN KEY(customer_id)
		REFERENCES customers(id)
		ON DELETE CASCADE
);
DELETE FROM customers WHERE email = 'george@gmail.com'; -- WHICH WORK NOW

CREATE TABLE students(
	id INT AUTO_INCREMENT PRIMARY KEY,
	first_name VARCHAR(50)
);

CREATE TABLE papers(
	title VARCHAR(50),
	grade INT,
	student_id INT,
	FOREIGN KEY(student_id) REFERENCES students(id)
);

INSERT INTO students (first_name) VALUES 
('Caleb'), ('Samantha'), ('Raj'), ('Carlos'), ('Lisa');

INSERT INTO papers (student_id, title, grade ) VALUES
(1, 'My First Book Report', 60),
(1, 'My Second Book Report', 75),
(2, 'Russian Lit Through The Ages', 94),
(2, 'De Montaigne and The Art of The Essay', 98),
(4, 'Borges and Magical Realism', 89);

SELECT first_name, title, grade
FROM students
JOIN papers
ON students.id = papers.student_id;

SELECT first_name, title, grade
FROM students
LEFT JOIN papers
	ON students.id = papers.student_id;
	
SELECT 
	first_name, 
	IFNULL(title,'MISSING') AS 'title', 
	IFNULL(grade,0) AS 'grade'
FROM students
LEFT JOIN papers
	ON students.id = papers.student_id;
	
SELECT 
	first_name, 
	IFNULL(AVG(grade),0) AS average
FROM students
LEFT JOIN papers
	ON students.id = papers.student_id
GROUP BY first_name;

SELECT 
	first_name, 
	IFNULL(AVG(grade),0) AS average,
	CASE 
		WHEN IFNULL(AVG(grade),0) >= 75 THEN 'PASSING'
		ELSE 'FAILING'
		END AS 'passing_status'
FROM students
LEFT JOIN papers
	ON students.id = papers.student_id
GROUP BY first_name
ORDER BY average DESC;
```

##### Many to Many Relationship

Example: TV Show Review

Series Data - Reviews Data - Reviewers Data

```mysql
CREATE TABLE reviewers (
	id INT AUTO_INCREMENT PRIMARY KEY,
	first_name VARCHAR(100),
	last_name VARCHAR(100)
);

CREATE TABLE series (
	id INT AUTO_INCREMENT PRIMARY KEY,
	title VARCHAR(100),
	released_year YEAR(4),
	genre VARCHAR(100)
);

CREATE TABLE reviews (
	id INT AUTO_INCREMENT PRIMARY KEY,
	rating DECIMAL(2,1),
	series_id INT,
	reviewer_id INT,
	FOREIGN KEY (series_id) REFERENCES series(id),
	FOREIGN KEY (reviewer_id) REFERENCES reviewers(id)
);

INSERT INTO series (title, released_year, genre) VALUES
    ('Archer', 2009, 'Animation'),
    ('Arrested Development', 2003, 'Comedy'),
    ("Bob's Burgers", 2011, 'Animation'),
    ('Bojack Horseman', 2014, 'Animation'),
    ("Breaking Bad", 2008, 'Drama'),
    ('Curb Your Enthusiasm', 2000, 'Comedy'),
    ("Fargo", 2014, 'Drama'),
    ('Freaks and Geeks', 1999, 'Comedy'),
    ('General Hospital', 1963, 'Drama'),
    ('Halt and Catch Fire', 2014, 'Drama'),
    ('Malcolm In The Middle', 2000, 'Comedy'),
    ('Pushing Daisies', 2007, 'Comedy'),
    ('Seinfeld', 1989, 'Comedy'),
    ('Stranger Things', 2016, 'Drama');
 
 
INSERT INTO reviewers (first_name, last_name) VALUES
    ('Thomas', 'Stoneman'),
    ('Wyatt', 'Skaggs'),
    ('Kimbra', 'Masters'),
    ('Domingo', 'Cortes'),
    ('Colt', 'Steele'),
    ('Pinkie', 'Petit'),
    ('Marlon', 'Crafford');
	
INSERT INTO reviews(series_id, reviewer_id, rating) VALUES
    (1,1,8.0),(1,2,7.5),(1,3,8.5),(1,4,7.7),(1,5,8.9),
    (2,1,8.1),(2,4,6.0),(2,3,8.0),(2,6,8.4),(2,5,9.9),
    (3,1,7.0),(3,6,7.5),(3,4,8.0),(3,3,7.1),(3,5,8.0),
    (4,1,7.5),(4,3,7.8),(4,4,8.3),(4,2,7.6),(4,5,8.5),
    (5,1,9.5),(5,3,9.0),(5,4,9.1),(5,2,9.3),(5,5,9.9),
    (6,2,6.5),(6,3,7.8),(6,4,8.8),(6,2,8.4),(6,5,9.1),
    (7,2,9.1),(7,5,9.7),
    (8,4,8.5),(8,2,7.8),(8,6,8.8),(8,5,9.3),
    (9,2,5.5),(9,3,6.8),(9,4,5.8),(9,6,4.3),(9,5,4.5),
    (10,5,9.9),
    (13,3,8.0),(13,4,7.2),
    (14,2,8.5),(14,3,8.9),(14,4,8.9);
	
SELECT title, rating
FROM series
JOIN reviews
	ON series.id = reviews.series_id;
	
SELECT title,
	   AVG(rating) AS avg_rating
FROM series
JOIN reviews
	ON series.id = reviews.series_id
GROUP BY title
ORDER BY avg_rating;

SELECT 
	first_name,
	last_name,
	rating
FROM reviewers
JOIN reviews
	ON reviewers.id = reviews.reviewer_id;
	
SELECT 
	title AS unreviewed_series
FROM series
LEFT JOIN reviews
	ON series.id = reviews.series_id
WHERE rating IS NULL;

SELECT
	genre,
	ROUND(
		AVG(rating),
		2
	) AS avg_rating
FROM series
JOIN reviews
	ON series.id = reviews.series_id
GROUP BY genre;

SELECT
	first_name,
	last_name,
	COUNT(rating) AS COUNT,
	IFNULL(MIN(rating),0) AS MIN,
	IFNULL(MAX(rating),0) AS MAX,
	IFNULL(AVG(rating),0) AS AVG,
	
	IF(COUNT(rating) >= 1, 'ACTIVE','INACTIVE') AS STATUS
	
	#CASE
	#	WHEN COUNT(rating) = 0 THEN 'INACTIVE'
	#	ELSE 'ACTIVE'
	#	END AS 'STATUS'
FROM reviewers
LEFT JOIN reviews
	ON reviewers.id = reviews.reviewer_id
GROUP BY reviewers.id;

SELECT
	title,
	rating,
	CONCAT(first_name, ' ', last_name) AS reviewer
FROM series
JOIN reviews
	ON series.id = reviews.series_id
JOIN reviewers
	ON reviewers.id = reviews.reviewer_id
ORDER BY title;
```

##### Managing Data Example

```mysql
CREATE DATABASE ig_clone;
USE ig_clone;

CREATE TABLE users (
	id INT AUTO_INCREMENT PRIMARY KEY,
	username VARCHAR(255) UNIQUE,
	created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE photos (
	id INT AUTO_INCREMENT PRIMARY KEY,
	image_url VARCHAR(255) NOT NULL,
	user_id INT NOT NULL,
	created_at TIMESTAMP DEFAULT NOW(),
	FOREIGN KEY(user_id) REFERENCES users(id)
);

CREATE TABLE comments (
	id INT AUTO_INCREMENT PRIMARY KEY,
	comment_text VARCHAR(255) NOT NULL,
	user_id INT NOT NULL,
	photo_id INT NOT NULL,
	created_at TIMESTAMP DEFAULT NOW(),
	FOREIGN KEY(user_id) REFERENCES users(id),
	FOREIGN KEY(photo_id) REFERENCES photos(id)
);

CREATE TABLE likes (
	user_id INT NOT NULL,
	photo_id INT NOT NULL,
	created_at TIMESTAMP DEFAULT NOW(),
	FOREIGN KEY(user_id) REFERENCES users(id),
	FOREIGN KEY(photo_id) REFERENCES photos(id),
	PRIMARY KEY(user_id, photo_id)
);

CREATE TABLE follows (
	follower_id INT NOT NULL,
	followee_id INT NOT NULL,
	created_at TIMESTAMP DEFAULT NOW(),
	FOREIGN KEY(follower_id) REFERENCES users(id),
	FOREIGN KEY(followee_id) REFERENCES users(id),
	PRIMARY KEY(follower_id, followee_id)
);

CREATE TABLE tags (
	id INT AUTO_INCREMENT PRIMARY KEY,
	tag_name VARCHAR(255) NOT NULL
);
```

```mysql
 SELECT username, created_at from users
 ORDER BY created_at
 LIMIT 5;
 
 SELECT 
	DATE_FORMAT(created_at, '%a') AS day
 FROM users
 GROUP BY DATE_FORMAT(created_at, '%a')
 ORDER BY COUNT(*) DESC LIMIT 1;
 
 SELECT username
FROM users
LEFT JOIN photos
	ON photos.user_id = users.id
WHERE photos.id is NULL;

SELECT 
	username
FROM users
JOIN photos
	ON photos.user_id = users.id
WHERE photos.id =
(
SELECT
	photo_id
FROM likes
GROUP BY photo_id
ORDER BY COUNT(*) DESC LIMIT 1);

SELECT 
	AVG(total)
FROM(
SELECT
	username,
	photos.id,
	COUNT(photos.id) AS total
FROM users
LEFT JOIN photos
	ON photos.user_id = users.id
GROUP BY users.id
) AS count_total;

SELECT (SELECT COUNT(*) FROM photos) / (SELECT COUNT(*) FROM users) AS avg;

SELECT 
	tag_name,
	COUNT(*)
FROM photo_tags
-- JOIN photos
-- 	ON photos.id = photo_tags.photo_id
JOIN tags
	ON photo_tags.tag_id = tags.id
GROUP BY tags.id
ORDER BY COUNT(*) DESC LIMIT 5;

SELECT username
FROM (
SELECT 
	username,
	COUNT(*) as total
FROM users
JOIN likes
 	ON users.id = likes.user_id
GROUP BY username
) AS new_table
WHERE total = (SELECT COUNT(*) FROM photos);

SELECT 
	username,
	COUNT(*) as total
FROM users
JOIN likes
 	ON users.id = likes.user_id
GROUP BY username
HAVING total = (SELECT COUNT(*) FROM photos);
```



##### Plus: TRIGGER

```mysql
CREATE TABLE users (
	username VARCHAR(100),
	age INT
);


DELIMITER $$

CREATE TRIGGER must_be_adult
     BEFORE INSERT ON users FOR EACH ROW
     BEGIN
          IF NEW.age < 18
          THEN
              SIGNAL SQLSTATE '45000'
                    SET MESSAGE_TEXT = 'Must be an adult!';
          END IF;
     END;
$$

DELIMITER ;

DELIMITER $$

CREATE TRIGGER cannot_follow_self
     BEFORE INSERT ON followers FOR EACH ROW
     BEGIN
	 	IF NEW.follower_id = NEW.following_id
		THEN
			SIGNAL SQLSTATE '45000'
				SET MESSAGE_TEXT = 'Cannot follow yourself, silly';
		END IF;
     END;
$$

DELIMITER ;

CREATE TABLE unfollows (
	follower_id INT NOT NULL,
	followee_id INT NOT NULL,
	created_at TIMESTAMP DEFAULT NOW(),
	FOREIGN KEY(follower_id) REFERENCES users(id),
	FOREIGN KEY(followee_id) REFERENCES users(id),
	PRIMARY KEY(follower_id, followee_id)
);

DELIMITER $$
CREATE TRIGGER create_unfollow
	AFTER DELETE ON follows FOR EACH ROW
	BEGIN
		INSERT INTO unfollows(follower_id, followee_id)
		VALUES(OLD.follower_id, OLD.followee_id);
	END;
$$
DELIMITER $$

DELIMITER $$

CREATE TRIGGER create_unfollow
    AFTER DELETE ON follows FOR EACH ROW 
BEGIN
    INSERT INTO unfollows
    SET follower_id = OLD.follower_id,
        followee_id = OLD.followee_id;
END$$

DELIMITER ;

-- Listing Triggers
SHOW TRIGGERS;

-- Removing Triggers
DROP TRIGGER trigger_name;
```

