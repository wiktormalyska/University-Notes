Dział algorytmiki zajmujący się algorytmami i strukturami danych pozwalającymi efektywnie wykonać działania na obiektach geometrycznych, takich jak zbiory punktów, odcinków, wielokątów, okręgów

Najczęściej rozpatrywane są problemy na płaszczyźnie czy też przestrzeni
- kiedy 2 odcinki się przecinają
- kiedy punkt należy do wielokąta
- jak znaleźć otoczkę wypukłą zbioru punktów na płaszczyźnie
- jak stwierdzić czy istnieję przecinające się pary odcinków

#### Szukanie przecinających się par odcinków - czy jakakolwiek para odcinków w zbiorze się przecina
- dane - odcinki reprezentują krzywą na mapie - drogę lub rzekę
- problem - nakładając na siebie 2 mapy - dróg i rzek znajdź wszystkie przecięcia par odcinków
- rozwiązanie siłowe - weź każdą parę odcinków, oblicz czy się nie przecinają, jeśli tak to podaj ich punkt przecięcia - $O(n^2)$

#### Metoda zamiatania - przemyśleni