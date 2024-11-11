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