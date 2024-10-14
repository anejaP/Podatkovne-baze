# Osnovne poizvedbe s SELECT
Vsi filmi z oceno višjo od 8 in več kot 10000 glasovi
```sql
SELECT naslov,ocena
FROM film
WHERE ocena > 8 AND glasovi > 10000
ORDER BY ocena DESC, naslov;
```
Filmi z oceno zaokroženo na cela števila
```sql
SELECT naslov,ROUN(ocena) AS 'zaokrozena_ocena'
FROM film
WHERE ocena > 8 AND glasovi > 10000
ORDER BY ocena DESC, naslov;
```
