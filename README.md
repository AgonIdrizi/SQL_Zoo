# SQL_Zoo
I've compiled the solutions to all of all 10 levels on the [SQLZOO Tutoral](http://sqlzoo.net/wiki/SQL_Tutorial)

## Sections: 
1. [SELECT from WORLD](#select-from-world)
2. [SELECT from NOBEL](#select-from-nobel)
3. [SELECT in SELECT](#select-in-select)
4. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)


## SELECT from WORLD

1.
```sql
SELECT name, continent, population FROM world
```
2.
```sql
SELECT name FROM world
WHERE population >=200000000
```
3.
```sql
SELECT name,(gdp/population) AS 'gdp/population' FROM world
WHERE population >=200000000
```
4.
```sql
SELECT name, population/1000000 from world
where continent = 'South America'
```
5.
```sql
SELECT name , population FROM world
WHERE name IN ('France','Germany','Italy')
```
6.
```sql
SELECT name FROM world
WHERE name LIKE '%United%'
```
7.
```sql
SELECT name,population,area FROM world
where area > 3000000 or population>250000000
```
8.
```sql
SELECT name, population, area from world
WHERE area > 3000000 xor population>250000000
```
9.
```sql
SELECT name, ROUND(population/1000000,2) AS 'pop/mil', ROUND(GDP/1000000000,2) from world
where continent = 'South America'
```
10.
```sql
SELECT name, ROUND(gdp/population,-3) from world 
WHERE gdp >= 1000000000000
```
11.
```sql
SELECT name,  capital
  FROM world
 WHERE LENGTH(name) = LENGTH(capital)
```
12.
```sql
SELECT name, capital
FROM world
where LEFT(name,1) = LEFT(capital,1) and name <> capital
```
13.
```sql
SELECT name
   FROM world
WHERE name LIKE '%a%' and name LIKE '%e%' and name LIKE '%i%' and name LIKE '%o%' and name LIKE '%u%'
  AND name NOT LIKE '% %'
```
## SELECT from NOBEL

1.
```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```
2.
```sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'
```
3.
```sql
select yr, subject from nobel
where winner = 'Albert Einstein'
```
4.
```sql
SELECT winner FROM nobel
where yr>=2000 and subject = 'Peace'
```
5.
```sql
SELECT * from nobel
where yr >= 1980 and yr <=1989 and subject = 'Literature'
```
6.
```sql
SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Woodrow Wilson',
                  'Jimmy Carter',
'Barack Obama')
```
7.
```sql
SELECT winner from nobel
where winner LIKE 'John%'
```
8.
```sql
SELECT yr,subject, winner from nobel
where (subject = 'Physics' and yr = 1980) or (subject = 'Chemistry' and yr = 1984)
```
9.
```sql
SELECT yr, subject, winner from nobel
WHERE subject NOT IN ('Chemistry','Medicine') and yr =1980
```
10.
```sql
SELECT yr, subject, winner from nobel
WHERE (subject = 'Medicine' and yr<1910 ) or (subject = 'Literature' and yr>=2004)
```
11.
```sql
SELECT * FROM nobel
where winner = 'PETER GRÜNBERG'
```
12.
```sql
SELECT * FROM nobel
where winner = 'EUGENE O\'NEILL'
```
13.
```sql
SELECT winner, yr, subject FROM nobel
where winner LIKE "Sir%"
ORDER BY yr desc
```
14.
```sql
SELECT winner, subject
  FROM nobel
 WHERE yr=1984
 ORDER BY
 CASE WHEN subject IN ('Chemistry','Physics') THEN 1 ELSE 0 END,
subject, winner
```


## SELECT in SELECT

1.
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```
2.
```sql
select name  from world
where continent = 'Europe'
AND gdp/population>(Select gdp/population from world where name = 'United Kingdom')
```
3.
```sql
SELECT name , continent from world
where continent IN (SELECT continent from world where name ='Argentina' or name ='Australia')
ORDER BY name
```
4.
```sql
SELECT name,population from world
where population < 
               (SELECT population from world where name = 'Poland') and population > (SELECT population from world where name = 'Canada')
```
5.
```sql
SELECT name,
        CONCAT(ROUND(100*population/(SELECT population 
                                    FROM world 
                                    WHERE name='Germany'))
       ,'%')
FROM world
WHERE continent = 'Europe'
```
6.
```sql
SELECT name from world
where gdp>ALL(SELECT gdp
                     from world
                     where continent = 'Europe' and gdp>0)
```
7.
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
```
8.
```sql
SELECT continent, name FROM world x WHERE name <= ALL(SELECT name FROM world y WHERE x.continent = y.continent);
```
9.
```sql
SELECT name, continent, population FROM world WHERE continent IN (SELECT continent FROM world  x WHERE 25000000 >= (SELECT MAX(population) FROM world y WHERE x.continent = y.continent));
or
SELECT y.name, y.continent, y.population
FROM world AS y
JOIN
(SELECT continent,max(population)
FROM world
GROUP BY continent
HAVING max(population) <= 25000000) AS x
ON y.continent = x.continent
```

10.
```sql
SELECT x.name, x.continent
FROM world AS x
WHERE x.population/3 > ALL (
  SELECT y.population
  FROM world AS y
  WHERE x.continent = y.continent
  AND x.name != y.name);
```
## SUM and COUNT

1.
```sql
SELECT SUM(population)
FROM world
```
2.
```sql
SELECT DISTINCT continent from world
```
3.
```sql
select SUM(gdp) from world
where continent = 'Africa'
```
4.
```sql
select COUNT(name) from world
where area>1000000
```
5.
```sql
select SUM(population) from world
where name IN ('Estonia', 'Latvia', 'Lithuania')
```
6.
```sql
select continent, COUNT(name) from world
GROUP BY continent
```
7.
```sql
select continent, COUNT(name) from world
where population >10000000
GROUP BY continent
```
8.
```sql
select continent from world
GROUP BY continent
HAVING SUM(population)>100000000
```

## JOIN

1.
```sql
SELECT matchid, player FROM goal 
  WHERE teamid = 'GER'
```
2.
```sql
SELECT id,stadium,team1,team2
  FROM game
where id =1012
```
3.
```sql
SELECT goal.player,goal.teamid, game.stadium, game.mdate
  FROM game 
   JOIN goal 
   ON (game.id = goal.matchid)
where goal.teamid = 'GER'
```
4.
```sql
select team1, team2, goal.player
from game
JOIN goal
ON game.id = goal.matchid
where player LIKE 'Mario%'
```
5.
```sql
SELECT player, teamid,eteam.coach, gtime
  FROM goal 
JOIN eteam
ON goal.teamid = eteam.id
where gtime<10
```
6.
```sql
select game.mdate, eteam.teamname
from game
JOIN eteam
ON team1 = eteam.id
where coach = 'Fernando Santos'
```
7.
```sql
select goal.player
from goal
join game
ON goal.matchid = game.id
where stadium = 'National Stadium, Warsaw'
```
8.
```sql
SELECT DISTINCT goal.player
  FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER' ) AND (goal.teamid <>'GER')
```
9.
```sql
SELECT teamname, COUNT(teamid)
  FROM eteam JOIN goal ON id=teamid
GROUP BY teamname
```
10.
```sql
select game.stadium , COUNT(teamid)
from game
JOIN goal
ON game.id = goal.matchid
GROUP BY stadium
```
11.
```sql
SELECT matchid,mdate, COUNT(teamid)
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid
```
12.
```sql
select goal.matchid, game.mdate, COUNT(teamid)
from game
JOIN goal
ON game.id = goal.matchid
where(teamid='GER')
GROUP BY goal.matchid
```
## More JOIN

1.
```sql
SELECT id, title
 FROM movie
 WHERE yr=1962
```
2.
```sql
select yr from movie
where title = 'Citizen Kane'
```
3.
```sql
select id,title,yr from movie
where title LIKE '%Star Trek%'
```
4.
```sql
select id from actor
where name = 'Glenn Close'
```
5.
```sql
select id from movie
where title = 'Casablanca'
```
6.
```sql
select actor.name from casting
JOIN actor
on actorid = id
where movieid = 11768
```
7.
```sql
select actor.name from casting
JOIN actor
on actorid = id
where movieid = (select id from movie where title = 'Alien')
```
8.
```sql
select title from movie
JOIN casting
ON movie.id = casting.movieid
where casting.actorid = (select id from actor where name = 'Harrison Ford')
```
9.
```sql
select title from movie
JOIN casting
ON movie.id = casting.movieid
where casting.actorid = (select id from actor where name = 'Harrison Ford') and casting.ord <>1
```
10.
```sql
select movie.title, actor.name from movie
JOIN casting
ON movie.id = casting.movieid
JOIN actor
ON casting.actorid = actor.id

where movie.yr = 1962 and casting.ord=1
```
11.
```sql
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
where name='John Travolta'
GROUP BY yr
HAVING COUNT(title)=(SELECT MAX(c) FROM
(SELECT yr,COUNT(title) AS c FROM
   movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
 where name='John Travolta'
 GROUP BY yr) AS t
)
```
12.
```sql
SELECT title, name
  FROM movie JOIN casting ON (movieid = movie.id
                               AND ord=1)
             JOIN actor ON (actorid = actor.id)
  WHERE movie.id IN (
        SELECT movieid FROM casting
         WHERE actorid IN (
            SELECT id FROM actor
             WHERE name = 'Julie Andrews'))   
```
13.
```sql
SELECT name
FROM actor
  JOIN casting ON (id = actorid AND (SELECT COUNT(ord) FROM casting WHERE actorid = actor.id AND ord=1)>=30)
GROUP BY name
```
14.
```sql
SELECT title, COUNT(actorid) as cast
FROM movie JOIN casting on id=movieid
WHERE yr = 1978
GROUP BY title
ORDER BY cast DESC
```
15.
```sql
SELECT DISTINCT name
FROM actor JOIN casting ON id=actorid
WHERE movieid IN (SELECT movieid FROM casting JOIN actor ON (actorid=id AND name='Art Garfunkel')) AND name != 'Art Garfunkel'
GROUP BY name
```

## Using NULL

1.
```sql
select name from teacher
where dept is NULL
```
2.
```sql
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)
```
3.
```sql
SELECT teacher.name, dept.name
 FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)
```
4.
```sql
SELECT teacher.name, dept.name
 FROM teacher RIGHT JOIN dept
           ON (teacher.dept=dept.id)
```
5.
```sql
select name,COALESCE(mobile,'07986 444 2266') AS mobile
 from teacher
```
6.
```sql
select teacher.name, COALESCE(dept.name,'None')
from teacher
LEFT OUTER JOIN dept
ON teacher.id = dept.id
```
7.
```sql
select COUNT(name), COUNT(mobile) from teacher
```
8.
```sql
SELECT dept.name, COUNT(teacher.name)
FROM teacher RIGHT JOIN dept
ON teacher.dept=dept.id
GROUP BY dept.name
```
9.
```sql
SELECT name, CASE WHEN dept IN (1,2) 
THEN 'Sci'
ELSE 'Art' END
FROM teacher
```
## Self JOIN
1.
```sql
SELECT DISTINCT COUNT(*) FROM stops
```
2.
```sql
SELECT id FROM stops
  WHERE name = 'Craiglockhart'
```
3.
```sql
SELECT id, name FROM stops JOIN route ON (stops.id = route.stop)
  WHERE num = 4
```
4.
```sql
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING COUNT(*) = 2
```
5.
```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=53 AND b.stop = (SELECT id FROM stops WHERE name = 'London Road')
```
6.
```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' and stopb.name = 'London Road'
```
7.
```sql
SELECT a.company, a.num  
FROM route a, route b
WHERE a.num = b.num AND (a.stop = 115 AND b.stop = 137)
GROUP BY num;
```
8.
```sql
SELECT a.company, a.num
FROM route a
JOIN route b ON (a.company = b.company AND a.num = b.num)
JOIN stops stopa ON a.stop = stopa.id
JOIN stops stopb ON b.stop = stopb.id
WHERE stopa.name = 'Craiglockhart'
AND stopb.name = 'Tollcross';
```
9.
```sql
SELECT DISTINCT name, a.company, a.num
FROM route a
JOIN route b ON (a.company = b.company AND a.num = b.num)
JOIN stops ON a.stop = stops.id
WHERE b.stop = 53;
```
10.
```sql
SELECT a.num, a.company, stopb.name, c.num, c.company
FROM route a
JOIN route b ON (a.company = b.company AND a.num = b.num)
JOIN (route c JOIN route d ON (c.company = d.company AND c.num = d.num))
JOIN stops stopa ON a.stop = stopa.id
JOIN stops stopb ON b.stop = stopb.id
JOIN stops stopc ON c.stop = stopc.id
JOIN stops stopd ON d.stop = stopd.id
WHERE stopa.name = 'Craiglockhart'
	AND stopd.name = 'Sighthill'
	AND stopb.name = stopc.name
ORDER BY LENGTH(a.num), b.num, stopb.name, LENGTH(c.num), d.num;
```
