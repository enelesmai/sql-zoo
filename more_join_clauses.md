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

    SELECT name
    FROM casting 
    JOIN actor ON actor.id = actorid
    JOIN movie ON movie.id = movieid
    WHERE title = 'Alien'

2. List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

    SELECT title
    FROM casting 
    JOIN actor ON actor.id = actorid
    JOIN movie ON movie.id = movieid
    WHERE name = 'Harrison Ford' and ord <> 1

3. List the films together with the leading star for all 1962 films.

    SELECT title, name
    FROM casting 
    JOIN actor ON actor.id = actorid
    JOIN movie ON movie.id = movieid
    WHERE yr = 1962 and ord = 1
