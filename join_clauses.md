Exercises taken from: https://sqlzoo.net/

Original data:

```
game
id	    mdate	        stadium	                    team1	team2
1001	8 June 2012	    National Stadium, Warsaw	POL	    GRE
1002	8 June 2012	    Stadion Miejski (Wroclaw)	RUS	    CZE
1003	12 June 2012	Stadion Miejski (Wroclaw)	GRE	    CZE
1004	12 June 2012	National Stadium, Warsaw	POL	    RUS
```

```
goal
matchid	teamid	player	                gtime
1001	POL	    Robert Lewandowski	    17
1001	GRE	    Dimitris Salpingidis	51
1002	RUS	    Alan Dzagoev	        15
1002	RUS	    Roman Pavlyuchenko	    82
```

```
eteam
id	teamname	    coach
POL	Poland	        Franciszek Smuda
RUS	Russia	        Dick Advocaat
CZE	Czech Republic	Michal Bilek
GRE	Greece	        Fernando Santos
```

1. You can combine the two steps into a single query with a JOIN.

    SELECT *
    FROM game JOIN goal ON (id=matchid)

The FROM clause says to merge data from the goal table with that from the game table. The ON says how to figure out which rows in game go with which rows in goal - the matchid from goal must match id from game. (If we wanted to be more clear/specific we could say
    ON (game.id=goal.matchid)

The code below shows the player, teamid, stadium and mdate for every German goal.

    SELECT player,teamid,stadium,mdate
    FROM game JOIN goal ON (id=matchid)
    WHERE goal.teamid = 'GER'

2. Show the name of all players who scored a goal against Germany.

    SELECT DISTINCT player
    FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER')
    AND teamid <> 'GER'

3. Show teamname and the total number of goals scored.

    SELECT teamname, count(matchid)
    FROM eteam JOIN goal ON id=teamid
    GROUP BY teamid
    ORDER BY teamname

4. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.

Notice in the query given every goal is listed. If it was a team1 goal then a 1 appears in score1, otherwise there is a 0. You could SUM this column to get a count of the goals scored by team1. Sort your result by mdate, matchid, team1 and team2.

    SELECT 
        mdate,
        team1,
        SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
        team2,
        SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
    FROM game LEFT JOIN goal ON matchid = id
    GROUP BY mdate, matchid, team1, team2
    ORDER BY mdate, matchid, team1, team2
    