
# PHP&MYSQL -  POŁĄCZENIE Z BAZĄ

## ✅ Połączenie z bazą
OBIEKTOWO
```php
$conn = new mysqli(
    "localhost",
    'root',
    '',
    'szkola'
);
```
PROCEDURALNIE
```php
$conn = mysqli_connect(
    "localhost",
    'root',
    '',
    'szkola'
);
```

### Sprawdzenie połącznia (opcjonalnie)
```php
$conn = @new mysqli(
    "localhost",
    'root',
    '',
    'szkola'
);

if($conn->connect_errno){
    echo "Nie połączono z bazą danych <br />
    Opis:  {$conn->connect_error} <br />
    Kod: {$conn->connect_errno}
    ";
    exit();
}

```


## ✅ Zapytanie SQL - wykonanie/pobranie danych


```php
// Zmienne, które przchowują przykładowe dane
$imie = "Adama";
$nazwisko = "Mickiewicz";
```
OBIEKTOWO:

```php
#METODA 1
$sql = "SELECT Imie, Nazwisko, Ksiazka FROM biblioteka WHERE Imie='{$imie}' AND Nazwisko='{$nazwisko}'";
$res = $conn->query($sql);

#METODA 2
$sql = "SELECT Imie, Nazwisko, Ksiazka FROM biblioteka WHERE Imie = ? AND nazwisko =?";
$query = $conn->prepare($sql);
$query->bind_param('ss', $imie, $nazwisko);
$query->execute();
$res = $query->get_result();

/*
type:
u - decimal
s - stinrg
i - int
d - double
b - blob
*/
```

PROCEDURALNIE:

```php
#METODA 1
$sql = "SELECT Imie, Nazwisko, Ksiazka FROM biblioteka WHERE Imie='{$imie}' AND Nazwisko='{$nazwisko}'";
$res = $mysqli_query($conn, $sql);

```




## ✅ Wyświetalnie danych z bazy PĘTLA!
OBIEKTOWO: \
(pętla foreach działa tak samo jak pętla while z feych_assoc)
```php
// TABLICA ASOCJACYJNA, PO NAZWIE
foreach($res as $row){
    echo "
    <p>{$row['Imie']}</p>
    <p>{$row['Nazwisko']}</p>
    <p>{$row['Ksiazka']}</p>
    ";
}

while($row = $res->fetch_assoc()){
    echo "
    <p>{$row['Imie']}</p>
    <p>{$row['Nazwisko']}</p>
    <p>{$row['Ksiazka']}</p>
    ";
}

//TABLICA NUMERYCZNA, PO INDEXIE
while($row = $res->fetch_row()){
    echo "
    <p>{$row[0]}</p>
    <p>{$row[1]}</p>
    <p>{$row[2]}</p>
    ";
}

echo " <h1>Liczba zwróconych wierszy: {$res->num_rows}</h1>";
echo " <h1>Liczba zwróconych kolumn: {$res->num_fileds}</h1>";
```

PROCEDURALNIE:

```php
// TABLICA ASOCJACYJNA, PO NAZWIE
while($row = mysqli_fetch_assoc($res)){
    echo "
    <p>{$row['Imie']}</p>
    <p>{$row['Nazwisko']}</p>
    <p>{$row['Ksiazka']}</p>
    ";
}

//TABLICA NUMERYCZNA, PO INDEXIE
while($row = mysqli_fetch_row($res)){
    echo "
    <p>{$row[0]}</p>
    <p>{$row[1]}</p>
    <p>{$row[2]}</p>
    ";
}

echo " <h1>Liczba zwróconych wierszy: " . mysqli_num_rows($res) . "</h1>";
echo " <h1>Liczba zwróconych kolumn: " . mysqli_num_fileds($res) . "</h1>";
```




## ✅ Zamknięcie połącznia


OBIEKTOWO:
```php
$conn->close();
```

PROCEDURALNIE:
```php
mysqli_close($conn);
```


\
Przyklad pliku polaczenie_obiektowe.php:
```php

$conn = new mysqli(
    "localhost",
    'root',
    '',
    'szkola'
);

$imie = "Adama";
$nazwisko = "Mickiewicz";

$sql = "SELECT Imie, Nazwisko, Ksiazka FROM biblioteka WHERE Imie='{$imie}' AND Nazwisko='{$nazwisko}'";
$res = $conn->query($sql);

foreach($res as $row){
    echo "
    <p>{$row['Imie']}</p>
    <p>{$row['Nazwisko']}</p>
    <p>{$row['Ksiazka']}</p>
    ";
}

$conn->close();
```

Przyklad pliku polaczenie_proceduralne.php:
```php

$conn = mysqli_connect("localhost",'root','','szkola');

$imie = "Adama";
$nazwisko = "Mickiewicz";

$sql = "SELECT Imie, Nazwisko, Ksiazka FROM biblioteka WHERE Imie='{$imie}' AND Nazwisko='{$nazwisko}'";
$res = mysqli_query($conn, $sql);

while($row = mysqli_fetch_assoc($res)){
    echo "
    <p>{$row['Imie']}</p>
    <p>{$row['Nazwisko']}</p>
    <p>{$row['Ksiazka']}</p>
    ";
}


mysqli_close($conn);
```