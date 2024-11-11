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
    ime        text       NOT NULL,
    priimek    text       NOT NULL,
    email      text       NOT NULL
);

CREATE TABLE predmeti (
    id        integer    PRIMARY KEY,  
    ime       text       NOT NULL,
    ects      integer    NOT NULL
);

ALTER TABLE ucitelji 
ADD COLUMN kabinet text;

CREATE TABLE vloge (
    id        integer    PRIMARY KEY    CHECK (id IN (0, 1)),
    opis      text       NOT NULL
);

CREATE TABLE izvajalci (
    idpredmeta    integer    REFERENCES predmeti(id),
    iducitelja    integer    REFERENCES ucitelji(id),
    vloga         integer    REFERENCES vloge(id)
);

SELECT kabinet, COUNT(*) as stevilo_uciteljev
FROM ucitelji
GROUP BY kabinet
ORDER BY stevilo_uciteljev DESC;

SELECT ime, priimek 
FROM ucitelji
```