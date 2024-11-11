# Ustvarjanje tabel
```sql
CREATE TABLE stranka (
    id        integer    PRIMARY KEY,
    ime       text       NOT NULL
);

CREATE TABLE narocilo (
    id        integer    PRIMARY KEY,
    kolicina  integer    CHECK ( kolicina > 0),
    stranka   integer    REFERENCES stranka (id),
    status    varchar(10) CHECK (status IN ('oddano','v obdelavi', 'na poti', 'zaključeno')) DEFAULT 'oddano'
);

INSERT INTO stranka
(ime)
VALUES ('Alenka'), ('Branko'),('Cvetka'),('David');

INSERT INTO narocilo
(kolicina,stranka,status)
VALUES (500,2,'v obdelavi'),
       (300,3,'na poti'),
       (800,2,'v obdelavi'),
       (150,1,'oddano'),
       (400,4,'zaključeno'),
       (400,1,'na poti');
```

Baza uciteljev
```sql
CREATE TABLE ucitelji (
    id         integer    PRIMARY KEY,
    ime        text,
    priimek    text,
    email      text 
);

CREATE TABLE predmeti (
    id    integer    PRIMARY KEY,
    ime   text,
    ects  integer
);

ALTER TABLE ucitelji 
ADD COLUMN kabinet text;

CREATE TABLE vloge (
    id    integer    PRIMARY KEY    CHECK (id IN (0,1)),
    opis  text
);

CREATE TABLE izvajalci (
    idpredmeta    integer    REFERENCES predmeti(id),
    iducitelja    integer    REFERENCES ucitelji(id),
    vloga         integer    REFERENCES vloge(id)
);

-- vnesli smo podatke v drugih editorjih in sicer podatke iz spletne

SELECT kabinet, COUNT(*) AS stevilo_uciteljev
FROM ucitelji
GROUP BY kabinet
ORDER BY stevilo_uciteljev DESC;

SELECT ucitelji.ime AS ime1, 
       ucitelji.priimek AS priimek1, 
       drugi_ucitelji.ime AS ime2, 
       drugi_ucitelji.priimek AS priimek2
    FROM ucitelji
    JOIN ucitelji AS drugi_ucitelji 
        ON ucitelji.kabinet = drugi_ucitelji.kabinet AND ucitelji.id < drugi_ucitelji.id
ORDER BY ucitelji.kabinet;


SELECT predmeti.ime AS predmet, 
       ucitelji.ime AS ime_ucitelja, 
       ucitelji.priimek AS priimek_ucitelja,
       asistenti.ime AS ime_asistenta, 
       asistenti.priimek AS priimek_asistenta
FROM izvajalci
JOIN ucitelji ON izvajalci.iducitelja = ucitelji.id AND izvajalci.vloga = 0
JOIN izvajalci AS izvajalci_asistenti ON izvajalci.idpredmeta = izvajalci_asistenti.idpredmeta AND izvajalci_asistenti.vloga = 1
JOIN ucitelji AS asistenti ON izvajalci_asistenti.iducitelja = asistenti.id
JOIN predmeti ON izvajalci.idpredmeta = predmeti.id;
```