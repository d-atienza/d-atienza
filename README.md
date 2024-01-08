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




