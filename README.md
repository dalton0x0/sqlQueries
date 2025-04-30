# Exemple de requêtes SQL avec jointures

### 1. Créer la base de données
```sh
CREATE DATABASE IF NOT EXISTS Bibliothèques;
```

### 2. Créer les tables
```sh
DROP TABLE IF EXISTS Auteur;
DROP TABLE IF EXISTS Livre;
DROP TABLE IF EXISTS Emprunt;

CREATE TABLE Auteur (
    id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    nom VARCHAR(255),
    prenom VARCHAR(255),
    nationalite VARCHAR(255)
);
CREATE TABLE Livre (
    id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    titre VARCHAR(255),
    esbn INT NOT NULL,
    annee INT NOT NULL,
    editeur VARCHAR(255),
    idAuteur INT,
    FOREIGN KEY (idAuteur) REFERENCES Auteur(id)
);
CREATE TABLE Emprunt (
    id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    date_emprunt INT NOT NULL,
    date_retour INT,
    emprunteur VARCHAR(100),
    idLivre INT,
    FOREIGN KEY (idLivre) REFERENCES Livre(id)
);
```

### 3. Insérer les valauers dans la base données
```sh
INSERT INTO Auteur (nom, prenom, nationalite)
VALUES
('MALEK', 'Chaouch', 'French'),
('RIHAN', 'Adnan', 'English'),
('MESLE', 'Alexandre', 'Moroccan');

INSERT INTO Livre (titre, esbn, annee, editeur, idAuteur)
VALUES
('Java Spring Boot', '1213456789', '2025', 'Tech School', 1),
('PHP Laravel', '987456321', '2000', 'ITIC Paris', 2),
('Analyse & Merise', '846279135', '1990', 'ITIC Paris', 3);

INSERT INTO Emprunt (date_emprunt, date_retour, emprunteur, idLivre)
VALUES
('2024-03-01', '2024-03-07', 'Cheridanh', 3),
('2024-05-09', '2024-06-07', 'Loic', 1),
('2024-03-01', NULL, 'Valentin', 2);
```
## 4. Requêtes

### Sélectionner tous les auteurs :
```sh
SELECT * FROM Auteur;
```

### Sélectionner tous les livres publiés après l'année 2000 :
```sh
SELECT * FROM Livre WHERE annee > 2000;
```

### Sélectionner les emprunts où la date de retour est manquante (NULL) :
```sh
SELECT * FROM emprunt WHERE date_retour IS NULL;
```

### Compter le nombre total de livres dans la bibliothèque :
```sh
SELECT COUNT(*) AS total_livres FROM Livres;
```

### Sélectionner les éditeurs distincts dans la table Livre :
```sh
SELECT DISTINCT editeur FROM Livres;
```

### Sélectionner tous les livres avec les informations de leur auteur (INNER JOIN) :
```sh
SELECT Livre.titre, Auteur.nom, Auteur.prenom
FROM Livres
INNER JOIN Auteur ON Livre.idAuteur = Auteur.id;
```

### Sélectionner tous les auteurs, même ceux qui n'ont pas de livres associés (LEFT JOIN) :
```sh
SELECT Auteur.nom, Auteur.prenom, Livre.titre
FROM Auteur
LEFT JOIN Livre ON Auteur.id = Livre.idAuteur;
```

### Sélectionner tous les livres, même ceux sans auteur associé (RIGHT JOIN) :
```sh
SELECT Livre.titre, Auteur.nom, Auteur.prenom
FROM Auteur
RIGHT JOIN Livre ON Auteur.id = Livre.idAuteur;
```

### Sélectionner tous les livres et tous les auteurs, même s'il n'y a pas de correspondance (FULL JOIN) :
```sh
SELECT Livre.titre, Auteur.nom, Auteur.prenom
FROM Livre
LEFT JOIN Auteur ON Livre.idAuteur = Auteur.id
UNION
SELECT Livre.titre, Auteur.nom, Auteur.prenom
FROM Livre
RIGHT JOIN Auteur ON Livre.idAuteur = Auteur.id;
```

### Sélectionner les emprunts avec les informations du livre et de l'auteur (double jointure) :
```sh
SELECT Emprunt.date_emprunt, Emprunt.emprunteur, Livre.titre, Auteur.nom, Auteur.prenom
FROM Emprunt
INNER JOIN Livre ON Emprunt.idLivre = Livre.id
INNER JOIN Auteur ON Livre.idAuteur = Auteur.id;
```

### Compter le nombre de livres par auteur :
```sh
SELECT COUNT(Livre.titre) AS nombre_livres, Auteur.nom, Auteur.prenom
FROM AUTEUR
LEFT JOIN Livre ON Auteur.id = Livre.idAuteur
GROUP BY Auteur.id;
```

### Compter le nombre d'emprunts par livre :
```sh
SELECT Livre.titre, COUNT(Emprunt.id) AS nombre_emprunts
FROM Livre
LEFT JOIN Emprunt ON Livre.id = Emprunt.id
GROUP BY Livre.id;
```

### Compter le nombre de livres par éditeur :
```sh
SELECT editeur, COUNT(*) AS nombre_livres
FROM Livre
GROUP BY Livre.id;
```

### Sélectionner les nationalités distinctes des auteurs :
```sh
SELECT DISTINCT nationalite FROM Auteur;
```

### Sélectionner les années distinctes de publication des livres :
```sh
SELECT DISTINCT annee FROM Livre;
```

### Sélectionner les livres empruntés plus de 3 fois :
```sh
SELECT Livre.titre, COUNT(Emprunt.id) AS nombre_emprunts
FROM Livre
INNER JOIN Emprunt on Livre.id = Emprunt.idLivre
HAVING nombre_emprunts  
```

### Sélectionner les auteurs qui n'ont aucun livre associé :
```sh
SELECT Auteur.nom, Auteur.prenom
FROM Auteur
INNER JOIN Livre ON Auteur.id = Livre.idAuteur
WHERE Livre.titre IS NULL;
```

### Sélectionner les livres qui n'ont jamais été empruntés :
```sh
SELECT Livre.titre
FROM Livre
INNER JOIN Emprunt ON Livre.id = Emprunt.idLivre
WHERE Emprunt.id IS NULL;
```
