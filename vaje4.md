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
```