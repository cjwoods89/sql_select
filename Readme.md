1) Create a list of students showing first and last names only.

    mysql> SELECT first_name, last_name from student;
    +------------+-----------+
    | first_name | last_name |
    +------------+-----------+
    | Eric       | Ephram    |
    | Greg       | Gould     |
    | Adam       | Ant       |
    | Howard     | Hess      |
    | Charles    | Caldwell  |
    | James      | Joyce     |
    | Doug       | Dumas     |
    | Kevin      | Kraft     |
    | Frank      | Fountain  |
    | Brian      | Biggs     |
    +------------+-----------+
10 rows in set (0.00 sec)

2)Create a list of students with all the columns but only if the student has less than 8 years experience

    mysql> SELECT * FROM student WHERE years_of_experience < 8;
    +------------+------------+-----------+---------------------+------------+
    | student_id | first_name | last_name | years_of_experience | start_date |
    +------------+------------+-----------+---------------------+------------+
    |          1 | Eric       | Ephram    |                   2 | 2016-03-31 |
    |          2 | Greg       | Gould     |                   6 | 2016-09-30 |
    |          3 | Adam       | Ant       |                   5 | 2016-06-02 |
    |          4 | Howard     | Hess      |                   1 | 2016-02-28 |
    |          5 | Charles    | Caldwell  |                   7 | 2016-05-07 |
    |          8 | Kevin      | Kraft     |                   3 | 2016-04-15 |
    |         10 | Brian      | Biggs     |                   4 | 2015-12-25 |
    +------------+------------+-----------+---------------------+------------+
    7 rows in set (0.00 sec)

3) Create a list of students showing the lastname, startdate, and id columns only and sorted by last_name in ascending sequence

    mysql> SELECT last_name, start_date, student_id FROM student ORDER BY last_name ASC;
    +-----------+------------+------------+
    | last_name | start_date | student_id |
    +-----------+------------+------------+
    | Ant       | 2016-06-02 |          3 |
    | Biggs     | 2015-12-25 |         10 |
    | Caldwell  | 2016-05-07 |          5 |
    | Dumas     | 2016-07-04 |          7 |
    | Ephram    | 2016-03-31 |          1 |
    | Fountain  | 2016-01-31 |          9 |
    | Gould     | 2016-09-30 |          2 |
    | Hess      | 2016-02-28 |          4 |
    | Joyce     | 2016-08-27 |          6 |
    | Kraft     | 2016-04-15 |          8 |
    +-----------+------------+------------+
10 rows in set (0.00 sec)

4) Create a list of students showing all columns but only if the student first name is Adam, James, or Frank and sort them in last_name descending sequence.

    mysql> SELECT * FROM student WHERE first_name IN ('Adam', 'James', 'Frank') ORDER BY last_name DESC;
    +------------+------------+-----------+---------------------+------------+
    | student_id | first_name | last_name | years_of_experience | start_date |
    +------------+------------+-----------+---------------------+------------+
    |          6 | James      | Joyce     |                   9 | 2016-08-27 |
    |          9 | Frank      | Fountain  |                   8 | 2016-01-31 |
    |          3 | Adam       | Ant       |                   5 | 2016-06-02 |
    +------------+------------+-----------+---------------------+------------+
    3 rows in set (0.00 sec)

5) Create a list of students showing firstname, lastname and startdate where the startdate is between Jan 1, 2016 and June 30, 2016 and order the list by descending start_date sequence.

    mysql> SELECT first_name, last_name, start_date FROM student WHERE start_date BETWEEN '2016-01-01' AND '2016-06-30' ORDER BY start_date DESC;
    +------------+-----------+------------+
    | first_name | last_name | start_date |
    +------------+-----------+------------+
    | Adam       | Ant       | 2016-06-02 |
    | Charles    | Caldwell  | 2016-05-07 |
    | Kevin      | Kraft     | 2016-04-15 |
    | Eric       | Ephram    | 2016-03-31 |
    | Howard     | Hess      | 2016-02-28 |
    | Frank      | Fountain  | 2016-01-31 |
    +------------+-----------+------------+
6 rows in set (0.00 sec)

Advanced Challenge

1) Create the grade table:

    CREATE TABLE grade (
        grade_id int(11) unsigned NOT NULL AUTO_INCREMENT,
        grade_value varchar(30),
        PRIMARY KEY (grade_id)
        );

2) Create a foreign key to it in the assignment:

    *** Had to modify the grade_id column in assignment to be unsigned in order to add the foreign key ***

    alter table assignment add column grade_id int(11) unsigned not null ;

    *** ------------------------------ ***

    CREATE INDEX idx_grade_id
      ON assignment(grade_id);

    ALTER TABLE assignment
      ADD CONSTRAINT idx_grade_id
      FOREIGN KEY (grade_id) REFERENCES grade(grade_id);

3) Add the following values: Incomplete, Complete and unsatisfactory, Complete and satisfactory, Exceeds expectations, and Not graded.

    INSERT INTO grade (grade_value) VALUES
         ('Incomplete'),
         ('Complete and unsatisfactory'),
         ('Complete and satisfactory'),
         ('Exceeds expectations'),
         ('Not graded');

4) Add at least 5 records to the assignment table:

    INSERT INTO assignment (student_id, assignment_nbr, class_id, grade_id) VALUES
        (1,2,2,1),
        (2,3,2,3),
        (3,2,2,4),
        (4,3,2,5),
        (5,5,2,2),
        (6,2,2,1),
        (7,3,2,3),
        (8,3,2,1),
        (9,2,2,4),
        (10,3,2,5);

5) Execute the following commands:

    a)  mysql> EXPLAIN assignment;
        +----------------+------------------+------+-----+---------+----------------+
        | Field          | Type             | Null | Key | Default | Extra          |
        +----------------+------------------+------+-----+---------+----------------+
        | assignment_id  | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
        | student_id     | int(11)          | NO   |     | NULL    |                |
        | assignment_nbr | int(11)          | NO   |     | NULL    |                |
        | class_id       | int(11)          | YES  |     | NULL    |                |
        | grade_id       | int(11) unsigned | NO   | MUL | NULL    |                |
        +----------------+------------------+------+-----+---------+----------------+
        5 rows in set (0.00 sec)

    b)  mysql> select * from assignment;
        +---------------+------------+----------------+----------+----------+
        | assignment_id | student_id | assignment_nbr | class_id | grade_id |
        +---------------+------------+----------------+----------+----------+
        |             1 |          1 |              2 |        2 |        1 |
        |             2 |          2 |              3 |        2 |        3 |
        |             3 |          3 |              2 |        2 |        4 |
        |             4 |          4 |              3 |        2 |        5 |
        |             5 |          5 |              5 |        2 |        2 |
        |             6 |          6 |              2 |        2 |        1 |
        |             7 |          7 |              3 |        2 |        3 |
        |             8 |          8 |              3 |        2 |        1 |
        |             9 |          9 |              2 |        2 |        4 |
        |            10 |         10 |              3 |        2 |        5 |
        +---------------+------------+----------------+----------+----------+
        10 rows in set (0.00 sec)

    c)  mysql> Explain grade;
        +-------------+------------------+------+-----+---------+----------------+
        | Field       | Type             | Null | Key | Default | Extra          |
        +-------------+------------------+------+-----+---------+----------------+
        | grade_id    | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
        | grade_value | varchar(30)      | YES  |     | NULL    |                |
        +-------------+------------------+------+-----+---------+----------------+
        2 rows in set (0.00 sec)

    d)  mysql> select * from grade;
        +----------+-----------------------------+
        | grade_id | grade_value                 |
        +----------+-----------------------------+
        |        1 | Incomplete                  |
        |        2 | Complete and unsatisfactory |
        |        3 | Complete and satisfactory   |
        |        4 | Exceeds expectations        |
        |        5 | Not graded                  |
        +----------+-----------------------------+
        5 rows in set (0.00 sec)
