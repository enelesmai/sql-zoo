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

1. List the continents that have a total population of at least 100 million.

        SELECT continent FROM 
        (SELECT continent, SUM(population) population
        FROM world
        GROUP BY continent) tbl
        WHERE population >= 100000000
    