# Združevalne funkcije

```sql
SELECT stolpci, MIN(stolpec1), MAX(stolpec2), AVG(stolpec3)
FROM tabela
WHERE pogoj
ORDER BY urejevalni_pogoj

SELECT stolpci, MIN(stolpec1), MAX(stolpec2), AVG(stolpec3)
FROM tabela
WHERE pogoj
ORDER BY urejevalni_pogoj

-- Povprečna ocena filmov iz leta 2000
SELECT AVG(ocena)
FROM filmi
WHERE leto = 2000;

-- Povprečna ocena filmov za vsako leto
SELECT leto, AVG(ocena)
FROM filmi
GROUP BY leto;

-- Povprečna ocena filmov za vsako leto, ko je bilo vsaj 5 filmov
SELECT leto, AVG(ocena), COUNT(*) AS st_filmov
FROM filmi
GROUP BY leto
HAVING st_filmov > 5;

-- Vrnite povprečno oceno filmov iz leta 2000, zaokroženo na 1 decimalko.
SELECT ROUND(AVG(ocena),1) AS pov_ocena
FROM filmi
WHERE leto = 2000;

-- Vrnite število filmov in povprečno oceno filmov vsakega režiserja. 
--Rezultat uredite od najbolj uspešnega (najvišja ocena) do najmanj uspešnega režiserja.
SELECT reziser, COUNT(*) AS st_filmov, AVG(ocena) AS povp_ocena
FROM filmi
GROUP BY reziser
ORDER BY povp_ocena DESC;

-- Vrnite certifikate, ki se pojavijo pri vsaj 100 filmih.
SELECT certifikat, COUNT(*)
FROM filmi
GROUP BY certifikat
HAVING COUNT(*) >= 100;
```