## Zadanie:

W głowie Partycji Logicznej wszystko zaczęło się układać w całość. Wiedza ta jednak była mocno zatrważająca. Według najnowszych obserwacji wkrótce do naszego Układu Słonecznego dotrze niezbyt przyjazna ludzkości asteroida i jedyne czego możemy być pewni to nieuchronna eksterminacja naszego rodzaju. Wszystko wskazywało na to, że władze wiedzą o bardzo poważnej katastrofie, która wkrótce ma się wydarzyć, ale najwyraźniej nie mają zamiaru dzielić się tą informacją ze światem.

Partycja przy okazji swoich ostatnich badań sporządziła listę n nazwisk osób, które mogłyby pomóc jej ujawnić międzynarodowy spisek i przerwać milczenie. Jednak przekonanie każdej z nich będzie wymagać xi minut. Tak się składa że pierwsza z osób, do której uda się Partycja zgodzi się współpracować prawie od razu (po 1 minucie), miło połechatana, że to właśnie ona została poproszona o pomoc na początku. Każda kolejna wymaga zawsze 1 minuty i dodatkowych xi minut przemnożonych przez liczbę osób przekonanych do tej pory. Partycja musi tak dobrać kolejność odwiedzin, by proces werbowania pomocników zajął jej jak najmniej czasu. W przypadku tego samego czasu przekonywania Partycja decyduje się pójść najpierw do osoby z nazwiskiem wcześniejszym leksykograficznie.

## Wejście:

W pierwszej linii wejścia znajduje się liczba naturalna t, czyli liczba zestawów danych. Każdy zestaw danych składa się z dwóch liczb naturalnych n - liczba nazwisk oraz p - liczba nazwisk, jakie należy wyświetlić na wyjściu. W kolejnych liniach znajdują się opisy osób. Na opis osoby składają się: nazwisko jako jednowyrazowy ciąg znaków oraz liczba naturalna x, oznaczająca liczbę minut potrzebnych standardowo do przekonania tej osoby do współpracy.

- 1 ≤ t ≤ 10
- 1 ≤ p ≤ n ≤ 50000
- 1 ≤ x ≤ 1000

## Wyjście:

Na wyjściu program dla każdego zestawu danych ma wypisać pierwszych p osób, jakie powinna odwiedzić Partycja oraz minimalny czas w minutach, jaki należy poświęcić na werbowanie pomocników. Wynik powinien być resztą z dzielenia przez 10^6+3.

**UWAGA!** W zadaniu nie można używać bibliotek STL.

### Przykład:

**Wejście:**
```
3
5 5
Adamczyk 3
Sikora 4
Szewczyk 1 
Pietrzak 1
Mazur 3
6 3
Wisniewski 5 
Wojcik 4 
Wojciechowska 5 
Wozniak 4 
Wrobel 4 
Walczak 5
4 4
Nowak 10 
Malinowski 10 
Duda 10 
Kowalska 10 
```

Wyjście:
```
Sikora Adamczyk Mazur Pietrzak Szewczyk
21
Walczak Wisniewski Wojciechowska
69
Duda Kowalska Malinowski Nowak
64
```
