
Exercises taken from: https://sqlzoo.net/

Original data:

```
    name	    continent	area	    population	gdp
    Afghanistan	Asia	    652230	    25500100	20343000000
    Albania	    Europe	    28748	    2831741	    12960000000
    Algeria	    Africa	    2381741	    37100000	188681000000
    Andorra	    Europe	    468	        78115	    3712000000
    Angola	    Africa	    1246700	    20609294	100990000000
```

1. Exclusive OR (XOR). Show the countries that are big by area (more than 3 million) or big by population (more than 250 million) but not both. Show name, population and area.

        SELECT name, population, area
        FROM world
        WHERE area > 3000000 XOR population > 250000000 

Result

```
    name	    population	area
    Australia	23545500	7692024
    Brazil	    202794000	8515767
    Canada	    35427524	9984670
    Indonesia	252164800	1904569
    Russia	    146000000	17125242
```

2. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.

Show per-capita GDP for the trillion dollar countries to the nearest $1000.

    SELECT name, ROUND((gdp/population), -3)
    FROM world
    WHERE (gdp/1000000000000)>1

Result:
```
    name	        ROUND((gdp/po..
    Australia	    66000
    Brazil	        11000
    Canada	        45000
    China	        6000
    France	        40000
    Germany	        42000
    India	        2000
    Italy	        33000
    Japan	        47000
    Mexico	        10000
    Russia	        14000
    South Korea	    22000
    Spain	        28000
    United Kingdom	39000
    United States	51000
```

3. The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1.

Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.

    SELECT winner, subject
    FROM nobel
    WHERE yr=1984
    ORDER BY subject IN ('Physics','Chemistry'), subject, winner

Result:
```
winner	            subject
Richard Stone	    Economics
Jaroslav Seifert	Literature
César Milstein	    Medicine
Georges J.F. Köhler	Medicine
Niels K. Jerne	    Medicine
Desmond Tutu	    Peace
Bruce Merrifield	Chemistry
Carlo Rubbia	    Physics
Simon van der Meer	Physics
```
