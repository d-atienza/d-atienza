This repo is to store notes from my SQL exercises so that they are easily accessible to me wherever!

**DDL Student Records** 
Syntax:
CREATE TABLE student (
	matric_no CHAR(8) PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  date_of_birth DATE
);

INSERT INTO student (matric_no, first_name, last_name, date_of_birth)
VALUES
('40001010', 'Daniel', 'Radcliffe', '1989-07-23'), 
('40001011', 'Emma', 'Watson', '1990-04-15'),
('40001012', 'Rupert', 'Grint', '1988-10-24');

CREATE TABLE module (
	module_code CHAR(8) PRIMARY KEY,
  module_title VARCHAR(50),
  level INTEGER,
  credits INTEGER
);

INSERT INTO module (module_code, module_title, level, credits)
VALUES
('HUF07101', 'Herbology', 07, 20),
('SLY07102', 'Defense Against the Dark Arts', 07, 20),
('HUF08102', 'History of Magic', 08, 20);


CREATE TABLE registration (
  matric_no CHAR(8),
	module_code CHAR(8),
  result DECIMAL (4,1),
  PRIMARY KEY (matric_no, module_code)
);

INSERT INTO registration (matric_no, module_code, result)
VALUES
('40001010', 'SLY07102', 90),
('40001010', 'HUF07101', 40),
('40001010', 'HUF08102', 0.0),

('40001011', 'SLY07102', 99),
('40001011', 'HUF07101', 40),
('40001011', 'HUF08102', 0.0),

('40001012', 'SLY07102', 20),
('40001012', 'HUF07101', 40),
('40001012', 'HUF08102', NULL);


SELECT registration.module_code, student.first_name, student.last_name, registration.result,
CASE
 	WHEN registration.result <= 39 THEN 'F'
 	WHEN registration.result >= 40 AND result <= 69 THEN 'P' 
 	WHEN registration.result >= 70 THEN 'M'
 END AS grade
FROM registration
JOIN student
	ON registration.matric_no = student.matric_no
 WHERE registration.module_code = 'SLY07102';
// https://sqlzoo.net/wiki/DDL_Student_Records

----
**Using 'IN' Clause**
Q: Show the name and population for France, Germany, Italy
Syntax:
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');

----
**Using XOR Clause**
Q:Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.
Synax:
SELECT name, population, area
FROM world
WHERE area > 3000000 XOR population > 250000000;

----
**Using Round to the nearest 1000 instead of decimal place**
Q: Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.
Syntax:
SELECT name, ROUND(GDP/population, -3)
FROM world
WHERE GDP > 1000000000000

----
**How to use length in a query**
Q: Greece has capital Athens. Each of the strings 'Greece', and 'Athens' has 6 characters. Show the name and capital where the name and the capital have the same number of characters.
Syntax:
SELECT name, capital
  FROM world
 WHERE LENGTH(name) = LENGTH(capital);

----
**Using LEFT to isolate characters in string and <> as a NOT EQUALS instead of !=**
Q: The capital of Sweden is Stockholm. Both words start with the letter 'S'.
Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
- You can use the function LEFT to isolate the first character. You can use <> as the NOT EQUALS operator.

Syntax:
SELECT name, capital
FROM world
WHERE name <> capital AND LEFT(name,1) = LEFT(capital,1);

----
**Using NOT LIKE to filter out two worded countries (ie. United Kingdom)**
Q: Equatorial Guinea and Dominican Republic have all of the vowels (a e i o u) in the name. They don't count because they have more than one word in the name. Find the country that has all the vowels and no spaces in its name.
Syntax: 
SELECT name
   FROM world
WHERE name LIKE '%a%' 
AND name LIKE '%e%'
AND name LIKE '%i%'
AND name LIKE '%o%'
AND name LIKE '%u%'
AND name NOT LIKE '% %'; 

----
**Using 'VIEW' to create a mini table that you can reference if you are going to use that specific information constantly instead of using SELECT statement with all the clauses again and again**
Q: Create a 'view' of the course id and the number of students who have signed up for it
Syntax:
CREATE TABLE enrollment (
	student_id INTEGER PRIMARY KEY,
  course_id CHAR(5) NOT NULL
);

INSERT INTO enrollment (student_id, course_id)
VALUES (413011, 'CS101'), 
(612123, 'BI102'), 
(102542, 'CS101'), 
(231705, 'CS101'), 
(341024, 'MT103');


CREATE VIEW course_size AS
SELECT course_id, count(*) AS num
FROM enrollment
GROUP BY course_id;

SELECT * from course_size;

----
**Using subquery to query information from your original query** 
Q: Create a subquery / subselect of the average of the top scores of each team
Syntax:
CREATE TABLE moosesport (
	player VARCHAR(100),
	team VARCHAR(100),
	score INTEGER
);

----
INSERT INTO moosesport (player, team, score)
VALUES
('MARTHA', 'Ice Weasels', 17),
('BILLY BOY', 'Frostbiters', 23),
('JOJO', 'Ice Weasels', 11),
('MOOSE', 'Frostbiters', 41),
('MOOSE', 'Traffic Stoppers', 36),
('MOOSARINA', 'Traffic Stoppers', 12);

SELECT AVG(bigscore) AS maxes
FROM (SELECT MAX(score) AS bigscore
FROM moosesport
GROUP BY team) AS subquery;

// http://www.postgresql.org/docs/9.4/static/functions-subquery.html

----
**Using IN instead of OR**
Q: How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.
Syntax:
SELECT *
FROM cd.facilities
WHERE facid in (1, 5);

----
**Using DISTINCT removes doubles from a listing**
Q: How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.
Syntax:
SELECT distinct surname
FROM cd.members
ORDER BY surname ASC
LIMIT 10;

----
**Using a NESTED SELECT and the clause IN to sort information**
Q: List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
Syntax:
select name, continent
from world 
where continent in (select continent from world where name = 'Argentina' or name = 'Australia')
order by name asc;

----
**Using CONCAT to add on a percentage '%' and ROUND the decimal numbers to become integers**
Q: Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.

Syntax:
select name, concat(round(population/(select population from world where name = 'Germany')*100), '%') as percentage
from world
where continent = 'Europe';

----
**Using JOIN and paretheses to pool together an OR statement**
Q: Show the name of all players who scored a goal against Germany.
Syntax:
SELECT distinct player
  FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER') AND teamid!='GER'

----
**Using 2 JOINS to complete this!**
Q: Obtain the cast list for 'Casablanca'. There are 3 tables: actor, movie, casting
Syntax:
select actor.name
from movie

inner join casting
on movie.id = casting.movieid

inner join actor
on actorid = actor.id

where movie.title = 'Casablanca'
