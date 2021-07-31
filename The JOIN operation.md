# 地址

https://sqlzoo.net/wiki/The_JOIN_operation/zh

# 相关表结构

![](https://raw.githubusercontent.com/wryu2010/image/master/img/20210731195546.png)

![](https://raw.githubusercontent.com/wryu2010/image/master/img/20210731195620.png)

![](https://raw.githubusercontent.com/wryu2010/image/master/img/20210731195641.png)

# 相关题目

1. **列出 賽事編號 matchid 和球員名 player ,該球員代表德國隊Germany入球的。要找出德國隊球員，要檢查: `teamid = 'GER'` ** 

```mysql
SELECT matchid, player FROM goal 
WHERE teamid = 'GER'
```



2. **由以上查詢，你可見Lars Bender's 於賽事 1012入球。列出此賽事的對賽隊伍是哪一隊。**


```mysql
SELECT id,stadium,team1,team2
FROM game where id = 1012
```



3. **顯示每一個德國入球的球員名，隊伍名，場館和日期**。

```mysql
SELECT player,teamid,stadium,mdate
FROM game a
JOIN goal b ON b.matchid = a.id
WHERE b.teamid = 'GER'
```



4. **列出球員名字叫Mario (**`player LIKE 'Mario%'`**)有入球的 隊伍1 team1, 隊伍2 team2 和 球員名 player**

```mysql
SELECT team1,team2,player
FROM game a
JOIN goal b ON b.matchid = a.id
WHERE b.player LIKE 'Mario%'
```



5. **列出每場球賽中首10分鐘**`gtime<=10`**有入球的球員** `player`**, 隊伍**`teamid`**, 教練**`coach`**, 入球時間**`gtime`


```mysql
SELECT player, teamid, coach, gtime
FROM goal 
JOIN eteam ON id=teamid
WHERE gtime<=10
```



6. **列出'Fernando Santos'作為隊伍1 team1 的教練的賽事日期，和隊伍名。**


```mysql
SELECT mdate,teamname 
FROM game JOIN eteam ON team1=eteam.id
WHERE coach = 'Fernando Santos'
```



7. **列出場館 'National Stadium, Warsaw'的入球球員。**


```mysql
SELECT player FROM goal
LEFT JOIN game ON id = goal.matchid
WHERE stadium = 'National Stadium, Warsaw'
```



8. **列出全部賽事，射入德國龍門的球員名字。**

```mysql
SELECT distinct player FROM game 
JOIN goal ON matchid = id 
WHERE (team1='GER' OR team2='GER') and goal.teamid != 'GER'
```



9. **列出隊伍名稱** **teamname** **和該隊入球總數**

```mysql
SELECT teamname, count(*)
FROM eteam JOIN goal ON id=teamid
GROUP BY teamname
ORDER BY teamname;
```



10. **列出場館名和在該場館的入球數字。**


```mysql
SELECT stadium,count(1)
FROM game JOIN goal ON matchid = id
GROUP BY stadium
```



11. **每一場波蘭'POL'有參與的賽事中，列出賽事編號 matchid, 日期date 和入球數字。**

```mysq
SELECT matchid, mdate, count(1)
FROM game JOIN goal ON matchid = id 
WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid,mdate
```



12. **每一場德國'GER'有參與的賽事中，列出賽事編號 matchid, 日期date 和德國的入球數字。**

```mysq
SELECT matchid, mdate, count(1)
FROM game JOIN goal ON id=matchid
WHERE (team1='GER' OR team2='GER') AND goal.teamid = 'GER'
GROUP BY matchid, mdate
```



13. **List every match with the goals scored by each team as shown.** 

    ![](https://raw.githubusercontent.com/wryu2010/image/master/img/20210731211117.png)

```mysq
SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
FROM game LEFT JOIN goal ON matchid = id
GROUP BY mdate,matchid,team1,team2
ORDER BY mdate,matchid,team1, team2
```