Solutions to SQL Zoo exercises
# SQL_Zoo
# SELECT FROM WORLD


SELECT FROM WORLD

1.
SELECT name, continent, population FROM world

2.
SELECT name FROM world
WHERE population >=200000000

3.
SELECT name,(gdp/population) AS 'gdp/population' FROM world
WHERE population >=200000000

4.
SELECT name, population/1000000 from world
where continent = 'South America'

5.
SELECT name , population FROM world
WHERE name IN ('France','Germany','Italy')

6.
SELECT name FROM world
WHERE name LIKE '%United%'

7.
SELECT name,population,area FROM world
where area > 3000000 or population>250000000

8.
SELECT name, population, area from world
WHERE area > 3000000 xor population>250000000

9.
SELECT name, ROUND(population/1000000,2) AS 'pop/mil', ROUND(GDP/1000000000,2) from world
where continent = 'South America'

10.
SELECT name, ROUND(gdp/population,-3) from world 
WHERE gdp >= 1000000000000

11.
SELECT name,  capital
  FROM world
 WHERE LENGTH(name) = LENGTH(capital)

12.
SELECT name, capital
FROM world
where LEFT(name,1) = LEFT(capital,1) and name <> capital

13.
SELECT name
   FROM world
WHERE name LIKE '%a%' and name LIKE '%e%' and name LIKE '%i%' and name LIKE '%o%' and name LIKE '%u%'
  AND name NOT LIKE '% %'

SELECT FROM NOBEL

1.
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950

2.
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'

3.
select yr, subject from nobel
where winner = 'Albert Einstein'

4.
SELECT winner FROM nobel
where yr>=2000 and subject = 'Peace'

5.
SELECT * from nobel
where yr >= 1980 and yr <=1989 and subject = 'Literature'

6.
SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Woodrow Wilson',
                  'Jimmy Carter',
'Barack Obama')

7.
SELECT winner from nobel
where winner LIKE 'John%'

8.
SELECT yr,subject, winner from nobel
where (subject = 'Physics' and yr = 1980) or (subject = 'Chemistry' and yr = 1984)

9.
SELECT yr, subject, winner from nobel
WHERE subject NOT IN ('Chemistry','Medicine') and yr =1980

10.
SELECT yr, subject, winner from nobel
WHERE (subject = 'Medicine' and yr<1910 ) or (subject = 'Literature' and yr>=2004)

11.
SELECT * FROM nobel
where winner = 'PETER GRÜNBERG'

12.
SELECT * FROM nobel
where winner = 'EUGENE O\'NEILL'

13.
SELECT winner, yr, subject FROM nobel
where winner LIKE "Sir%"
ORDER BY yr desc

14.
SELECT winner, subject
  FROM nobel
 WHERE yr=1984
 ORDER BY
 CASE WHEN subject IN ('Chemistry','Physics') THEN 1 ELSE 0 END,
subject, winner



SELECT WITHIN SELECT

1.
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')

2.
select name  from world
where continent = 'Europe'
AND gdp/population>(Select gdp/population from world where name = 'United Kingdom')

3.
SELECT name , continent from world
where continent IN (SELECT continent from world where name ='Argentina' or name ='Australia')
ORDER BY name

4.
SELECT name,population from world
where population < 
               (SELECT population from world where name = 'Poland') and population > (SELECT population from world where name = 'Canada')

5.
SELECT name,
        CONCAT(ROUND(100*population/(SELECT population 
                                    FROM world 
                                    WHERE name='Germany'))
       ,'%')
FROM world
WHERE continent = 'Europe'

6.
SELECT name from world
where gdp>ALL(SELECT gdp
                     from world
                     where continent = 'Europe' and gdp>0)

7.
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)

8.
SELECT continent, name FROM world x WHERE name <= ALL(SELECT name FROM world y WHERE x.continent = y.continent);

9.
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


10.
SELECT x.name, x.continent
FROM world AS x
WHERE x.population/3 > ALL (
  SELECT y.population
  FROM world AS y
  WHERE x.continent = y.continent
  AND x.name != y.name);

SUM_and_COUNT

1.
SELECT SUM(population)
FROM world

2.
SELECT DISTINCT continent from world

3.
select SUM(gdp) from world
where continent = 'Africa'

4.
select COUNT(name) from world
where area>1000000

5.
select SUM(population) from world
where name IN ('Estonia', 'Latvia', 'Lithuania')

6.
select continent, COUNT(name) from world
GROUP BY continent

7.
select continent, COUNT(name) from world
where population >10000000
GROUP BY continent

8.
select continent from world
GROUP BY continent
HAVING SUM(population)>100000000


The_JOIN_operation

1.
SELECT matchid, player FROM goal 
  WHERE teamid = 'GER'

2.
SELECT id,stadium,team1,team2
  FROM game
where id =1012

3.
SELECT goal.player,goal.teamid, game.stadium, game.mdate
  FROM game 
   JOIN goal 
   ON (game.id = goal.matchid)
where goal.teamid = 'GER'

4.
select team1, team2, goal.player
from game
JOIN goal
ON game.id = goal.matchid
where player LIKE 'Mario%'

5.
SELECT player, teamid,eteam.coach, gtime
  FROM goal 
JOIN eteam
ON goal.teamid = eteam.id
where gtime<10

6.
select game.mdate, eteam.teamname
from game
JOIN eteam
ON team1 = eteam.id
where coach = 'Fernando Santos'

7.
select goal.player
from goal
join game
ON goal.matchid = game.id
where stadium = 'National Stadium, Warsaw'

8.
SELECT DISTINCT goal.player
  FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER' ) AND (goal.teamid <>'GER')

9.
SELECT teamname, COUNT(teamid)
  FROM eteam JOIN goal ON id=teamid
GROUP BY teamname

10.
select game.stadium , COUNT(teamid)
from game
JOIN goal
ON game.id = goal.matchid
GROUP BY stadium

11.
SELECT matchid,mdate, COUNT(teamid)
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid

12.
select goal.matchid, game.mdate, COUNT(teamid)
from game
JOIN goal
ON game.id = goal.matchid
where(teamid='GER')
GROUP BY goal.matchid

More_JOIN_operations

1.
SELECT id, title
 FROM movie
 WHERE yr=1962

2.
select yr from movie
where title = 'Citizen Kane'

3.
select id,title,yr from movie
where title LIKE '%Star Trek%'

4.
select id from actor
where name = 'Glenn Close'

5.
select id from movie
where title = 'Casablanca'

6.
select actor.name from casting
JOIN actor
on actorid = id
where movieid = 11768

7.
select actor.name from casting
JOIN actor
on actorid = id
where movieid = (select id from movie where title = 'Alien')

8.
select title from movie
JOIN casting
ON movie.id = casting.movieid
where casting.actorid = (select id from actor where name = 'Harrison Ford')

9.
select title from movie
JOIN casting
ON movie.id = casting.movieid
where casting.actorid = (select id from actor where name = 'Harrison Ford') and casting.ord <>1

10.
select movie.title, actor.name from movie
JOIN casting
ON movie.id = casting.movieid
JOIN actor
ON casting.actorid = actor.id

where movie.yr = 1962 and casting.ord=1

11.
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

12.
SELECT title, name
  FROM movie JOIN casting ON (movieid = movie.id
                               AND ord=1)
             JOIN actor ON (actorid = actor.id)
  WHERE movie.id IN (
        SELECT movieid FROM casting
         WHERE actorid IN (
            SELECT id FROM actor
             WHERE name = 'Julie Andrews'))   

13.
SELECT name
FROM actor
  JOIN casting ON (id = actorid AND (SELECT COUNT(ord) FROM casting WHERE actorid = actor.id AND ord=1)>=30)
GROUP BY name

14.
SELECT title, COUNT(actorid) as cast
FROM movie JOIN casting on id=movieid
WHERE yr = 1978
GROUP BY title
ORDER BY cast DESC

15.
SELECT DISTINCT name
FROM actor JOIN casting ON id=actorid
WHERE movieid IN (SELECT movieid FROM casting JOIN actor ON (actorid=id AND name='Art Garfunkel')) AND name != 'Art Garfunkel'
GROUP BY name


USING_NULL

1.
select name from teacher
where dept is NULL

2.
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)

3.
SELECT teacher.name, dept.name
 FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)

4.
SELECT teacher.name, dept.name
 FROM teacher RIGHT JOIN dept
           ON (teacher.dept=dept.id)

5.
select name,COALESCE(mobile,'07986 444 2266') AS mobile
 from teacher

6.
select teacher.name, COALESCE(dept.name,'None')
from teacher
LEFT OUTER JOIN dept
ON teacher.id = dept.id

7.
select COUNT(name), COUNT(mobile) from teacher

8.
SELECT dept.name, COUNT(teacher.name)
FROM teacher RIGHT JOIN dept
ON teacher.dept=dept.id
GROUP BY dept.name

9.
SELECT name, CASE WHEN dept IN (1,2) 
THEN 'Sci'
ELSE 'Art' END
FROM teacher


