Exercises taken from: https://sqlzoo.net/

Original data:

```
name	    continent	area	    population	gdp
Afghanistan	Asia	    652230	    25500100	20343000000
Albania	    Europe	    28748	    2831741	    12960000000
Algeria	    Africa	    2381741	    37100000	188681000000
Andorra	    Europe	    468	        78115	    3712000000
Angola	    Africa	    1246700	    20609294	100990000000
...
```

1. Find the largest country (by area) in each continent, show the continent, the name and the area:

        SELECT continent, name, area FROM world x
        WHERE area >= ALL
            (SELECT area FROM world y
                WHERE y.continent=x.continent
                AND area>0)

The above example is known as a correlated or synchronized sub-query.

**Using correlated subqueries**

A correlated subquery works like a nested loop: the subquery only has access to rows related to a single record at a time in the outer query. The technique relies on table aliases to identify two different uses of the same table, one in the outer query and the other in the subquery.

One way to interpret the line in the WHERE clause that references the two table is “… where the correlated values are the same”.

In the example provided, you would say “select the country details from world where the population is greater than or equal to the population of all countries where the continent is the same”.

2. First country of each continent (alphabetically)
List each continent and the name of the country that comes first alphabetically.

        SELECT DISTINCT continent, 
        (SELECT name FROM world WHERE continent = x.continent LIMIT 1)
        FROM world x

Result:

```
    continent	    country
    Africa	        Algeria
    Asia	        Afghanistan
    Caribbean	    Antigua and Barbuda
    Eurasia	        Armenia
    Europe	        Albania
    North America	Belize
    Oceania	        Australia
    South America	Argentina
```

3. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.

        SELECT name, continent, population FROM world
        WHERE continent IN
        (SELECT DISTINCT continent FROM world x WHERE 25000000 >= ALL (SELECT population FROM world y WHERE y.continent = x.continent and population))

Result:
```
name	            continent	population
Antigua and Barbuda	Caribbean	86295
Australia	        Oceania	    23545500
Bahamas	            Caribbean	351461
Barbados	        Caribbean	285000
Cuba	            Caribbean	11167325
Dominica	        Caribbean	71293
Dominican Republic	Caribbean	9445281
Fiji	            Oceania	    858038
Grenada	            Caribbean	103328
Haiti	            Caribbean	10413211
Jamaica	            Caribbean	2717991
Kiribati	        Oceania	    106461
Marshall Islands	Oceania	    56086
Micronesia, 
Federated States of	Oceania	    101351
Nauru	            Oceania	    9945
New Zealand	        Oceania	    4538520
Palau	            Oceania	    20901
Papua New Guinea	Oceania	    7398500
Saint Lucia	        Caribbean	180000
Samoa	            Oceania	    187820
Solomon Islands	    Oceania	    581344
Tonga	            Oceania	    103036
Trinidad and Tobago	Caribbean	1328019
Tuvalu	            Oceania	    11323
Vanuatu	            Oceania	    264652
```

4. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.

        SELECT name, continent 
        FROM world x 
        WHERE population > 3*(SELECT MAX(population) FROM world y WHERE x.continent = y.continent AND x.name <> y.name)

Result:
```
name	continent
Australia	Oceania
Brazil	South America
Russia	Eurasia
```
