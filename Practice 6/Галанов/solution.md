# Курс "Базы данных"

## Задание 6
 
### 1.
```sql
SELECT EXTRACT(YEAR FROM P.BIRTHDATE), COUNT(DISTINCT P.PLAYER_ID), COUNT(*)
FROM PLAYERS P
         JOIN RESULTS R on P.PLAYER_ID = R.PLAYER_ID
         JOIN EVENTS E on E.EVENT_ID = R.EVENT_ID
         JOIN OLYMPICS O on O.OLYMPIC_ID = E.OLYMPIC_ID
WHERE O."year" = 2004
  AND R.MEDAL = 'GOLD'
GROUP BY EXTRACT(YEAR FROM p.BIRTHDATE); 
```
### 2.
```sql
SELECT EVENTS.EVENT_ID
FROM EVENTS
         JOIN RESULTS R
              ON EVENTS.EVENT_ID = R.EVENT_ID
WHERE IS_TEAM_EVENT = 0
  AND MEDAL = 'GOLD'
GROUP BY EVENTS.EVENT_ID
HAVING COUNT(*) > 1;
```
### 3.
```sql
SELECT DISTINCT PLAYERS.NAME, OLYMPIC_ID
FROM PLAYERS
         JOIN RESULTS ON PLAYERS.PLAYER_ID = RESULTS.PLAYER_ID
         JOIN EVENTS ON RESULTS.EVENT_ID = EVENTS.EVENT_ID;
```
### 4.
```sql
SELECT P.COUNTRY_ID,
       CAST(COUNT(CASE
                      WHEN P.NAME LIKE 'A%'
                          OR P.NAME LIKE 'a%'
                          OR P.NAME LIKE 'E%'
                          OR P.NAME LIKE 'e%'
                          OR P.NAME LIKE 'I%'
                          OR P.NAME LIKE 'i%'
                          OR P.NAME LIKE 'O%'
                          OR P.NAME LIKE 'o%'
                          OR P.NAME LIKE 'U%'
                          OR P.NAME LIKE 'u%' THEN 1
           END) as DOUBLE) * 100 / COUNT(*) AS PERCENT
FROM PLAYERS P
GROUP BY P.COUNTRY_ID
ORDER BY PERCENT DESC LIMIT 1;
```
### 5.
```sql
SELECT C.COUNTRY_ID, CAST(COUNT(*) AS DOUBLE) * 100 / C.POPULATION AS PERCENT
FROM RESULTS
         JOIN EVENTS E on E.EVENT_ID = RESULTS.EVENT_ID
         JOIN OLYMPICS O on O.OLYMPIC_ID = E.OLYMPIC_ID
         JOIN PLAYERS P on P.PLAYER_ID = RESULTS.PLAYER_ID
         JOIN COUNTRIES C on C.COUNTRY_ID = P.COUNTRY_ID
WHERE "year" = 2000
  AND IS_TEAM_EVENT = 1
GROUP BY C.COUNTRY_ID, C.POPULATION
ORDER BY PERCENT
LIMIT 5;
```