Exercises taken from: https://sqlzoo.net/

Original data:

```
movie
id	title	yr	director	budget	gross
```

```
actor
id	name
```

```
casting
movieid	actorid	ord
```

1. Obtain the cast list for the film 'Alien'

```
    SELECT name
    FROM casting 
    JOIN actor ON actor.id = actorid
    JOIN movie ON movie.id = movieid
    WHERE title = 'Alien'
```

2. List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

```
    SELECT title
    FROM casting 
    JOIN actor ON actor.id = actorid
    JOIN movie ON movie.id = movieid
    WHERE name = 'Harrison Ford' and ord <> 1
```

3. List the films together with the leading star for all 1962 films.

```
    SELECT title, name
    FROM casting 
    JOIN actor ON actor.id = actorid
    JOIN movie ON movie.id = movieid
    WHERE yr = 1962 and ord = 1
```

4. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```
    SELECT yr,COUNT(title) FROM
        movie JOIN casting ON movie.id=movieid
    JOIN actor   ON actorid=actor.id
    WHERE name='Rock Hudson'
    GROUP BY yr
    HAVING COUNT(title) > 2
```

5. List the film title and the leading actor for all of the films 'Julie Andrews' played in.

```
    SELECT title, name
    FROM casting
    JOIN movie ON movie.id = movieid
    JOIN actor ON actorid = actor.id
    WHERE movieid IN (
    SELECT movieid FROM casting
    WHERE actorid IN (SELECT id FROM actor
    WHERE name='Julie Andrews'))
    AND ord = 1
```
