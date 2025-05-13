# 2.1 INLEIDING

# 2.2 EEN DATABASE MAKEN MET SQL
## OPDRACHT 1
### 1
## OPDRACHT 2
### 6
## OPDRACHT 3
### 18
## OPDRACHT 4
### 31
### 32
### 33
### 34

# 2.3 DATA OPVRAGEN UIT DE DATABASE: QUERY'S
## OPDRACHT 5
### 35
### 36
### 37

## OPDRACHT 6
## OPDRACHT 7
## OPDRACHT 8
## OPDRACHT 9

# 2.4 RESULTATEN ORDENEN EN STATISTIEK BEDRIJVEN
## OPDRACHT 10
### 64
1: Geef een lijst van merken uit de tabel telefoons, waarbij de merken niet dubbel voorkomen en zijn gesorteerd op alfabetische volgorde.
2: Geef de minimale prijs uit de tabel aankopen voor die aankopen waarvan de code 's1' is. Geef de minimale prijs de naam (het alias) 'best'.
3: Geef de maximale prijs uit de tabel aankopen van die aankopen waarvan de code niet overeenkomt met 'a1' of 'a1'. Noem de maximale prijs 'duur'.
4: Geef een lijst van unieke plaatsnamen uit de tabel vrienden waarvan de naam eindigt op 'en' en sorteer die plaatsnamen op alfabetische volgorde.

### 65
### 66

## OPDRACHT 11
## OPDRACHT 12
## OPDRACHT 13
## OPDRACHT 14

# 2.5 MEER STATISTIEK: GROEPEREN
## OPDRACHT 15
### 94
```
SELECT merk, COUNT(*) AS N FROM telefoons
GROUP BY 1
ORDER BY 2 DESC
```

### 95
```
SELECT merk, COUNT(*) AS N FROM telefoons
GROUP BY 1
HAVING N = 2
ORDER BY 2 DESC
```

### 96
Uit de tabel aankopen wordt elke unieke code (welke staat voor een bepaalde telefoon) eenmaal weergegeven met in de tweede kolom de laagste prijs die voor deze bepaalde telefoon is betaald.

### 97

### 98
```
SELECT code, AVG(prijs) AS gemiddelde FROM aankopen
GROUP BY code HAVING gemiddelde < 600
```

## OPDRACHT 16
### 99
```
SELECT land, COUNT(id) as aantal FROM vrouwen
GROUP BY land
ORDER BY aantal DESC
```

### 100
```
SELECT land, COUNT(id) as aantal FROM vrouwen
GROUP BY land
HAVING aantal > 1
ORDER BY aantal DESC
```

### 101
```
SELECT AVG(leeftijd) as gemiddelde FROM vrouwen
WHERE land = 'Japan'
```

### 102
```
SELECT ROUND(AVG(leeftijd), 1) as gemiddelde FROM vrouwen
WHERE land = 'Japan'
```

### 103
```
SELECT land, ROUND(AVG(leeftijd), 2) as gemiddelde FROM vrouwen
GROUP BY 1
HAVING COUNT(land) > 1
ORDER BY 2 ASC
```

## OPDRACHT 17
### 104
```
SELECT artiest, COUNT(artiest) FROM top2000
WHERE ed2019 IS NOT NULL
GROUP BY 1
ORDER BY 2 DESC
```

### 105
```
SELECT artiest, COUNT(artiest) as aantal FROM top2000
GROUP BY artiest
HAVING aantal > 20
```

### 106
```
SELECT titel, COUNT(titel) as dubbel FROM top2000
GROUP BY titel
HAVING dubbel > 2
```

### 107
De uitkomst is een lijst artiesten die in de top 100 van de editie van 2021 hebben gestaan met minstens vier noteringen. In de tweede kolom staat het gemiddelde van hun top-100-noteringen.

### 108

### 109
Je maakt hier voor de HAVING niet gebruik van de gemiddelde notering, maar van het aantal noteringen in de top 100. Je vraagt dus iets wezenlijk anders.


# 2.6 SUBQUERY'S
## ODPRACHT 18
### 110
```
SELECT MIN(prijs) AS laagste FROM aankopen WHERE code = (
    SELECT code FROM telefoons WHERE merk = 'Samsung' AND type = 'Galaxy A23'
)
```

### 111
```
SELECT COUNT(id) AS N FROM aankopen WHERE code IN (
    SELECT code FROM telefoons WHERE merk = 'Xiaomi' OR merk = 'Oneplus'
)
```
Je kunt in de het WHERE-command in de subquery ook op beide merken zoeken door ze in een lijst (array) te zetten. Dat ziet er als volgt uit:
```
SELECT COUNT(id) AS N FROM aankopen WHERE code IN (
    SELECT code FROM telefoons WHERE merk IN ('Xiaomi', 'Oneplus')
)
```

### 112
```
SELECT COUNT(id) FROM aankopen WHERE prijs < 500 AND code IN (
    SELECT code FROM telefoons WHERE merk = 'Samsung'
)
```

### 113
```
SELECT merk, type FROM telefoons WHERE code IN (
    SELECT code FROM aankopen WHERE prijs < 500
)
```

### 114
```
SELECT merk, type FROM telefoons WHERE merk = (
    SELECT merk FROM telefoons WHERE type = 'Z flip 4'
)
```

### 115
```
SELECT merk, type FROM telefoons WHERE merk = (
    SELECT merk FROM telefoons WHERE type = 'Z flip 4'
) AND type != 'Z flip 4'
```

## OPDRACHT 19
### 116

### 117
Het is mogelijk dat de beide subquery's meerdere resultaten opleveren. In dat geval moet je IN gebruiken, anders kan de query niet worden uitgevoerd.

### 118
```
SELECT naam FROM vrienden WHERE id IN (
    SELECT id FROM aankopen WHERE code IN (
        SELECT code FROM telefoons WHERE merk = 'Xiaomi'
    )
)
```

### 119
```
SELECT merk, type FROM telefoons WHERE code IN (
    SELECT code FROM aankopen WHERE id = (
        SELECT id FROM vrienden WHERE naam = 'Ingo'
    )
)
```

## OPDRACHT 20
### 120
```
SELECT titel FROM top2000 WHERE artiest = 'R.E.M.' AND ed2021 = (
    SELECT MIN(ed2021) FROM top2000 WHERE artiest = 'R.E.M.'
)
```

### 121
```
SELECT titel FROM top2000 WHERE artiest = 'U2' AND ed2020 < (
    SELECT MIN(ed2020) FROM top2000 WHERE artiest = 'Maan'
)
```

### 122
```
SELECT artiest, titel FROM top2000 WHERE artiest IN (
    SELECT artiest FROM top2000 WHERE ed2020 <= 5
) 
ORDER BY 1, 2 ASC
```

### 123
```
SELECT artiest, COUNT(artiest) AS 'aantal liedjes' FROM top2000 WHERE artiest IN (
    SELECT artiest FROM top2000 WHERE ed2020 <= 5
) 
GROUP BY 1
ORDER BY 1
```

### 124
```
SELECT titel FROM top2000 WHERE ed2020 <= 1000 AND artiest IN (
    SELECT artiest FROM top2000 WHERE ed2020 <= 50 AND ed2019 IS NULL
)
```

### 125
```
SELECT DISTINCT artiest FROM top2000 WHERE ed2019 <= 100 AND artiest NOT IN (
    SELECT artiest FROM top2000 WHERE ed2018 <= 100
)
```

### 126
De query 
```
SELECT DISTINCT artiest FROM top2000 WHERE ed2019 <= 100 AND NOT ed2018 <= 100 
```
geeft ook die artiesten weer die in 2018 met een ander nummer in de top 100 stonden.

# EINDOPDRACHTEN MET QUERY'S OP BASIS VAN ÉÉN TABEL
### 127

### 128
```
DROP DATABASE IF EXISTS `voornamen`;
CREATE DATABASE `voornamen`;
USE `voornamen`;

CREATE TABLE `namen` (
    `positie` int NOT NULL,
    `naam` text NOT NULL,
    `aantal` int NOT NULL,
    `geslacht` varchar(1),
);
```

### 129
De positie komt sowieso tweemaal voor. Aantal en geslacht kunnen ook meerdere keren voorkomen. Alleen de naam zou uniek zijn, ware het niet dat sommige namen voor beide geslachten gebruikt kunnen worden (Anne, Charlie, ...)

### 130
De combinatie naam en geslacht is uniek. Deze twee attributen kunnen dus samen de primary key worden.

### 131
```
SELECT positie, naam, aantal FROM namen
WHERE positie <= 25 AND geslacht = 'V'
ORDER BY positie
```

### 132
Geef de namen en geslachten van alle registraties die tussen de 100.000 en 250.000 keer voorkomen.

### 133
```
SELECT SUM(aantal)/1000000 as aantal FROM namen
WHERE positie <= 100
GROUP BY geslacht
```

### 134
```
SELECT COUNT(DISTINCT naam) as aantal FROM namen
```

### 135
In de uitkomst van de query worden namen die gelijk zijn, op een bijzonder teken na (zoals een accent op een klinker), als verschillende namen gezien.

### 136
```
SELECT geslacht, COUNT(naam) AS aantal FROM namen
WHERE aantal > 50000
GROUP BY geslacht
```

### 137
Van de volgende twee query's
```
SELECT naam FROM namen WHERE naam LIKE 'Ren_'
SELECT naam FROM namen WHERE naam LIKE 'Ren%'
```
zal de tweede meer resultaten opleveren. Het procentteken geeft aan dat alle namen die beginnen met 'Ren' meegeteld zullen worden. In de eerste query worden alleen vierletterige namen beginnend met 'Ren' meegenomen worden in het resultaat.

### 138
```
SELECT ROUND(AVG(aantal)) AS gemiddeld_aantal_registraties FROM namen
WHERE geslacht = 'M'
AND naam LIKE 'M%'
```

# 2.7 MEERDERE TABELLEN BEVRAGEN
## ODPRACHT 21
### 139








## OPDRACHT 25
### 167
```
SELECT COUNT(titel) AS aantal FROM track
WHERE artiest_id = (
    SELECT artiest_id FROM artiest
    WHERE naam = 'Ed Sheeran'
)
```

### 168
```
SELECT naam, COUNT(track_id) as aantal FROM artiest, track
WHERE artiest.artiest_id = track.artiest_id
GROUP BY naam
HAVING aantal = 
(
    SELECT COUNT(titel) AS aantal FROM track
    WHERE artiest_id = (
        SELECT artiest_id FROM artiest
        WHERE naam = 'Ed Sheeran'
    )
)
```

### 169
```
SELECT uit, COUNT(track_id) as aantal FROM track, artiest
WHERE track.artiest_id = artiest.artiest_id
AND naam = 'ABBA'
GROUP BY uit
ORDER BY uit
```

### 170 

### 171
```
SELECT naam, titel FROM artiest, track
WHERE artiest.artiest_id = track.artiest_id
AND track_id IN (
    SELECT track_id FROM notering
    WHERE positie = 2000
    GROUP BY track_id 
    HAVING COUNT(track_id) = 2
)
```

## OPDRACHT 25
### 172
### 173
### 174
### 175
### 176