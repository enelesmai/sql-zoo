Exercises taken from: https://sqlzoo.net/

Original data:

```
teacher
id	dept    name	    phone	mobile
101	1	    Shrivell	2753	07986 555 1234
102	1	    Throd	    2754	07122 555 1920
103	1	    Splint	    2293	
104		    Spiregrain	3287	
105	2	    Cutflower	3212	07996 555 6574
106		    Deadyawn	3345	
```


```
dept
id	name
1	Computing
2	Design
3	Engineering
```

1. Note the INNER JOIN misses the teachers with no department and the departments with no teacher.

        SELECT teacher.name, dept.name
        FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)

2. Use a different JOIN so that all teachers are listed.

        SELECT teacher.name, dept.name
        FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)

3. Use a different JOIN so that all departments are listed.
        
        SELECT teacher.name, dept.name
        FROM teacher RIGHT JOIN dept
           ON (teacher.dept=dept.id)

4. Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. Show teacher name and mobile number or '07986 444 2266'

        SELECT teacher.name, COALESCE(mobile,'07986 444 2266')
        FROM teacher

5. Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.

        SELECT teacher.name, COALESCE(dept.name,'None')
        FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)

6. Use COUNT to show the number of teachers and the number of mobile phones.

        SELECT COUNT(name), COUNT(mobile)
        FROM teacher

7. Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.

        SELECT dept.name, COUNT(teacher.id)
        FROM teacher RIGHT JOIN dept ON dept.id = teacher.dept
        GROUP BY dept.name

8. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.

        SELECT 
            name, 
            CASE dept 
                WHEN 1 THEN 'Sci' 
                WHEN 2 THEN 'Sci' 
                WHEN 3 THEN 'Art' 
                ELSE 'None' END 
            FROM teacher 
