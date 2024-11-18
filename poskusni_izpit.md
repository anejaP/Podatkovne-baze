# Poskusni izpit SQL

Osnovne poizvedbe
```sql
-- Vrnite tabelo imen in priimkov vseh oseb, ki jim je ime Matija
SELECT * FROM osebe
WHERE ime = 'Matija';

-- Vrnite tabelo imen in priimkov vseh oseb, urejeno po priimku
SELECT ime, priimek FROM osebe
ORDER BY priimek;

-- Vrnite imena vseh predmetov na praktični matematiki (smer: 1PrMa)
SELECT ime FROM predmeti
WHERE smer = '1PrMa';

-- Vrnite vse podatke o skupinah z manj kot eno uro
SELECT * FROM skupine
WHERE ure <= 1;
```

Uporaba COUNT
```sql
-- Vrnite število vseh predmetov na posamezni smeri
SELECT smer,COUNT(ime) FROM predmeti
GROUP BY smer;

-- Vrnite imena tistih predmetov, ki se pojavljajo na več smereh
SELECT ime FROM predmeti
GROUP BY ime
HAVING COUNT(smer) > 1;
```

Uporaba SELECT WITHIN SELECT
```sql
-- Vrnite imena in vse pripadajoče smeri tistih predmetov, ki se pojavljajo na več smereh
SELECT ime, smer FROM predmeti
WHERE ime = (SELECT ime FROM predmeti GROUP BY ime HAVING COUNT(smer) > 1);
```

Uporaba JOIN
```sql
-- Vrnite skupno število ur vsake osebe
SELECT osebe.ime, osebe.priimek, COUNT(ure) 
FROM skupine JOIN osebe ON učitelj = osebe.id
GROUP BY učitelj;

-- Vrnite imena in priimke vseh predavateljev,
-- torej tistih, ki imajo kakšno skupino tipa P
SELECT osebe.ime, osebe.priimek
FROM skupine JOIN osebe ON učitelj = osebe.id
WHERE tip = 'P';

-- Vrnite imena in priimke vseh predavateljev,
-- ki izvajajo tako predavanja (tip P) kot vaje (tipa V ali L)
SELECT osebe.ime, osebe.priimek
FROM osebe JOIN skupine ON osebe.id = skupine.učitelj
GROUP BY učitelj
HAVING COUNT(tip) >= 2;


-- Vrnite imena in smeri vseh predmetov, ki imajo kakšen seminar
SELECT ime, smer 
FROM predmeti JOIN skupine ON predmeti.id = skupine.id
HAVING tip = 'S';
```

UPDATE DELETE INSERT
```sql
-- Vsem predmetom na smeri 2PeMa spremenite smer na PeMa
UPDATE predmeti
SET smer = 'PeMa'
WHERE smer = '2PeMa';

-- Izbrišite vse predmete, ki niso dodeljeni nobeni skupini
DELETE from predmeti
WHERE id = (SELECT id 
            FROM predmeti JOIN dodelitve ON predmeti.id = dodelitve.predmet 
            WHERE predmet IS NULL);

-- Za vsak predmet, ki se pojavi tako na prvi kot drugi stopnji
-- (smer se začne z 1 oz. 2), dodajte nov predmet z istim imenom
-- in smerjo 0Mate (stolpca id ne nastavljajte, ker se bo samodejno
-- določil)
INSERT INTO predmeti 
(ime, smer)
VALUES ((SELECT ime FROM predmeti GROUP BY ime HAVING COUNT(smer)>= 2), '0Mate');
```

Težje (mešano)
```sql
-- Za vsako smer izpišite število različnih oseb, ki na njej poučujejo
SELECT smer, COUNT(osebe.ime) AS št_učiteljev
FROM osebe JOIN skupine ON osebe.id = skupine.učitelj
            JOIN dodelitve ON skupine.id = dodelitve.skupina
            JOIN predmeti ON dodelitve.predmet = predmeti.id
GROUP BY smer;         

-- Vrnite pare ID-jev tistih oseb, ki sodelujejo pri vsaj dveh
-- predmetih (ne glede na tip skupine), pri čemer naj bo prvi ID
-- v paru manjši od drugega, pari pa naj se ne ponavljajo
SELECT DISTINCT(osebe.id, dodelitve.predmet)
FROM osebe JOIN skupine ON osebe.id = skupine.učitelj
            JOIN dodelitve ON skupine.id = dodelitve.skupina
            JOIN predmeti ON dodelitve.predmet = predmeti.id
GROUP BY osebe.id
HAVING COUNT(predmeti.ime) >= 2;

-- Za vsako osebo (izpišite jo z ID-jem, imenom in priimkom) vrnite
-- skupno število ur vaj (tako avditornih kot laboratorijskih),
-- pri čemer naj bo to enako 0, če oseba ne izvaja nobenih vaj

-- Vrnite ID-je, imena in smeri predmetov, za katere se izvaja
-- seminar, ne pa tudi avditorne ali laboratorijske vaje
```