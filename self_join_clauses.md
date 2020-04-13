Exercises taken from: https://sqlzoo.net/

Tables:

**stops**
```
id
name
```

**route**
```
num
company
pos
stop
```

1. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.

        SELECT a.company, a.num, a.stop, b.stop
        FROM route a JOIN route b ON
            (a.company=b.company AND a.num=b.num)
        WHERE a.stop=53 and b.stop = 149

2. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'

        SELECT a.company, a.num, stopa.name, stopb.name
        FROM route a JOIN route b ON
         (a.company=b.company AND a.num=b.num)
         JOIN stops stopa ON (a.stop=stopa.id)
        JOIN stops stopb ON (b.stop=stopb.id)
        WHERE stopa.name='Craiglockhart' AND stopb.name='London Road'

3. Find the routes involving two buses that can go from Craiglockhart to Lochend. Show the bus no. and company for the first bus, the name of the stop for the transfer, and the bus no. and company for the second bus.

        SELECT DISTINCT a.num, a.company, stopb.name ,  c.num,  c.company
        FROM route a JOIN route b
            ON (a.company = b.company AND a.num = b.num)
            JOIN ( route c JOIN route d ON (c.company = d.company AND c.num= d.num))
        JOIN stops stopa ON (a.stop = stopa.id)
        JOIN stops stopb ON (b.stop = stopb.id)
        JOIN stops stopc ON (c.stop = stopc.id)
        JOIN stops stopd ON (d.stop = stopd.id)
        WHERE  stopa.name = 'Craiglockhart' AND stopd.name = 'Lochend'
            AND  stopb.name = stopc.name
        ORDER BY a.num, a.company, stopb.name, c.num, c.company
