# 地址 

https://sqlzoo.net/wiki/SUM_and_COUNT

# 相关表结构

![](https://raw.githubusercontent.com/wryu2010/image/master/img/20210731193354.png)



# 相关题目

1. **展示世界的總人口。**

```mysql
SELECT sum(population) FROM world
```



2. **列出所有的洲份, 每個只有一次。**

```mysql
select distinct continent from world
```



3. **找出非洲(Africa)的GDP總和。**

```mysql
select sum(gdp) from world where continent = 'Africa'
```



4. **有多少個國家具有至少百萬(1000000)的面積。**

```mysql
select count(1) from world where area >= 1000000
```



5. **('France','Germany','Spain')（“法國”，“德國”，“西班牙”）的總人口是多少？**

```mysql
select sum(population) from world where name in('France','Germany','Spain')
```



6. **對於每一個洲份，顯示洲份和國家的數量。**

```mysql
select continent,count(1) from world group by continent
```



7. **對於每一個洲份，顯示洲份和至少有1000萬人(10,000,000)口國家的數目。**

```mysql
select continent,count(1) from world 
where population >= 10000000
group by continent
```



8. **列出有至少100百萬(1億)(100,000,000)人口的洲份。**

```mysql
select continent from world
group by continent 
having  sum(population) > 100000000
```



