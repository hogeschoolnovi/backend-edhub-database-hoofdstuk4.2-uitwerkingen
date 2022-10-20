# backend-edhub-hoofdstuk4.2-uitwerkingen

DROP TABLE IF EXISTS docent_cursus;
DROP TABLE IF EXISTS cursus CASCADE;
DROP TABLE IF EXISTS docent;
DROP TABLE IF EXISTS student;
DROP TABLE IF EXISTS resultaat;

CREATE TABLE docent (
    id INT GENERATED ALWAYS AS IDENTITY,
    naam TEXT NOT NULL UNIQUE, 
    geslacht CHAR (1) NOT NULL,
    in_dienst DATE NOT NULL,
    PRIMARY KEY (id)
);
    
CREATE TABLE cursus (
    id INT GENERATED ALWAYS AS IDENTITY,
    naam TEXT NOT NULL UNIQUE,
    ects INTEGER DEFAULT 5,
    hoofddocent_id INT NOT NULL,
    PRIMARY KEY (id),
    CONSTRAINT fk_hoofddocent FOREIGN KEY (hoofddocent_id) REFERENCES docent(id)
);

CREATE TABLE docent_cursus (
    docent_id INT,
    cursus_id INT,
    CONSTRAINT fk_docent FOREIGN KEY (docent_id) REFERENCES docent(id),
    CONSTRAINT fk_cursus FOREIGN KEY (cursus_id) REFERENCES cursus(id)
);

CREATE TABLE student (
	naam TEXT NOT NULL UNIQUE,
	geslacht CHAR (1) NOT NULL,
    cursus INT NOT NULL,
	PRIMARY KEY (naam),
	CONSTRAINT fk_cursus FOREIGN KEY (cursus) REFERENCES cursus(id)
);

CREATE TABLE resultaat (
	id INT GENERATED ALWAYS AS IDENTITY,
	deadline DATE NOT NULL,
	cijfer INT NOT NULL,
	cursus INT NOT NULL,
	PRIMARY KEY (id),
	CONSTRAINT fk_cursus FOREIGN KEY (cursus) REFERENCES cursus(id)
);

-- voeg het 4 ogen principe later nog toe door nog twee extra colommen toe te voegen

ALTER TABLE resultaat ADD COLUMN docent INT 
CONSTRAINT fk_docent REFERENCES docent(id);

ALTER TABLE resultaat ADD COLUMN examinator INT 
CONSTRAINT fk_examinator REFERENCES docent(id);
