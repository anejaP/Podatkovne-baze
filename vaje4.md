# Spreminjanje podatkov v tabeli

```sql
INSERT INTO stranka
VALUES (5,'Erik'), (6,'Fani'), (7,'Gala');

UPDATE narocilo
SET status = 'na poti'
WHERE id = 3;

INSERT INTO narocilo
(kolicina, stranka, status)
VALUES (200,(SELECT id FROM stranka WHERE ime = 'Gala'),'v obdelavi');

DELETE FROM narocilo
WHERE id = (SELECT id FROM stranka WHERE ime = 'Alenka');

INSERT INTO narocilo
(kolicina,stranka)
SELECT 100*id,id FROM stranka WHERE id NOT IN (SELECT DISTINCT stranka FROM narocilo);

UPDATE narocilo
SET status = 'zaključeno'
WHERE status = 'na poti';

SELECT * FROM stranka;

SELECT * FROM narocilo;

--V tabelo dodaj vsaj 5 filmov, posnetih po letu 2017. To naredi z enim ukazom INSERT INTO.
--Denimo, da smo posneli novo različico vseh filmov, posnetih pred letom 1950 s certifikatom G, 
--PG ali PG-13. Vstavi jih v tabelo, z ustrezno popravljenim naslovom, opisom in režiserjem. 
--Njihova dolžina naj bo za toliko daljša, kot je absolutna vrednost razlike med dolžino originalnega filma 
--in povprečjem dolžin teh (pred letom 1950, s certifikatom ...) filmov. 
--Njihova ocena naj bo za ena nižja od ocene originalnega filma. Kaj bi storili z id?

SELECT naslov || ' 2', 
        2024, 
        reziser, 
        certifikat,
        dolzina + ABS(dolzina-(SELECT AVG(dolzina) FROM filmi WHERE leto < 1950 AND certifikat IN ('G', 'PG', 'PG-13'))), 
        ocena-1, 
        opis
FROM filmi
WHERE leto < 1950 AND certifikat IN ('G', 'PG', 'PG-13');
```