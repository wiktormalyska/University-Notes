Mieszanie jest zupełnie odmiennym rozwiązaniem problemu słownika od [[Drzewo BST]]
Wykorzystuje się w nim własności numeryczne przechowywanych elementów (w [[Drzewo BST]] jedyna informacja o elementach pochodzi z wyników porównań)

Umiemy wykonać operację SEARCH w czasie logarytmicznym przy pomocy [[Drzewo AVL]] $O(\log n)$
Chcemy uzyskać czas stały $O(1)$

### Adresowanie bezpośrednie - direct access table
- Prosta metoda, skuteczna jeżeli uniwersum kluczy $U$ jest rozsądnie małe
- Zbiór dynamiczny o $n$ (unikalnych) kluczach można reprezentować za pomocą tablicy z adresowaniem bezpośrednim, w której każdej pozycji odpowiada klucz należący do uniwersum
![[Pasted image 20240613004102.png]]
- na pozycji $k$ znajduje się do elementu o kluczy $k$, lub cały element jest umieszczany w taj komórce tablicy
- trywialna implementacja operacji słownikowych - search, insert, delete - każda działa w czasie $O(1)$
- Problem 1 - klucze mogą nie być dodatkowymi liczbami całkowitymi
- Problem 2 - duże zużycie pamięci - jeżeli kluczy jest dużo lub chociaż niewiele, ale mają wartości z dużego zakresu

Chcemy skorzystać z małej tablicy w której za pomocą funkcji $h$ będziemy przechowywać klucze z $S$
Jeśli $h$ będzie odwzorowaniem losowym, to możemy liczyć na mało kolizji

#### Funkcja haszująca
Jest to funkcja, która każdemu elementowi ze zbioru dopuszczalnych kluczy przyporządkowuje liczbę całkowitą

Funkcja mieszająca jest funkcją odwzorowującą uniwersum, z którego pochodzą elementy zbioru $S$, w zbiór adresów dla pewnej liczby naturalnej $m$.
Dobra funkcja haszująca powinna:
- być łatwa w implementacji
- łatwo obliczalna (złożoność obliczeniowa)
- losowa - każdy indeks powinien być jednakowo prawdopodobny jako wartość funkcji
- bliskie klucze powinny dawać odległe wartości funkcji haszującej
- gdyby to dane umieszczane w tablicy haszującej były losowe wówczas jako wynik funkcji haszującej można wziąć wybrane bity z klucza

#### Tablica haszująca - Idea
- n-elementowa tablica rekordów postaci (key, value)
- h - funkcja haszująca
- wstawienie pary $(k,v)$ do tablicy haszującej to wykonanie przypisania $tab[(k)]=(k,v)$

#### Konflikty
- Obliczona wartość funkcji haszującej daje indeks w tablicy, pod którym można odnaleźć poszukiwany element
- Jeżeli okaże się że na pozycji o danym indeksie znajduje się kilka elementów zbioru $S$, mówimy o wystąpieniu zjawiska KOLIZJI

Kolizję nazywamy sytuację gdy dla różnych kluczy, funkcja h przyjmuje tę samą wartość

#### Rozwiązywanie konfliktów
- metoda łańcuchowa (haszowanie otwarte)
- adresowanie otwarte (haszowanie zamknięte)

##### Haszowanie otwarte (chaining)
- Elementy tablicy haszującej **tab** to listy wskaźnikowe przechowujące pary **(k,v)**.
- Element tablicy **tab** o indeksie $I$ zawiera listę przechowującą pary **(k,v)** takie, że $h(k)=I)$ a $h$ jest funkcją haszującą

##### Haszowanie zamknięte - adresowanie liniowe/kwadratowe
- m (ilość miejsc) $>=$ n (liczba elementów)
- w przypadku kolizji dokonaj rehaszowania czyli oblicz/znajdź kolejne miejsce, gdzie potencjalnie można umieścić zapisywany rekord
- ponawiaj próbę aż znajdziesz wolne miejsce

#### Haszowanie zamknięte - podwójne (double hashing)
- Zamiast przyrostu 1 (lub kwadrat) można wziąć przyrost określony przez drugą funkcję mieszającą $h`(v)$
- $r(i,k)=((h(k)+i*h`(k)))\mod m$
- Funkcja ta powinna spełniać następujące warunki
	- $h`(v)>0$
	- $h`(v)$ względnie pierwsza z $m$ (najlepiej gdy m jest liczbą pierwszą)
	- $h`$ istotnie różna od h, np. dla funkcji $h=k \mod m$ można wziąć funkcję $h`(k)=m-2-k \mod (m-2)$ gdzie $m$ jest liczbą pierwszą

##### Haszowanie zamknięte - DELETE(key)
- usuwając jakiś element z tablicy nie usuwamy go fizycznie - ryzyko utraty możliwości odnalezienia w tablicy innych elementów jeżeli zbyt wcześnie zakończymy rehaszowanie
- zamiast tego - oznacz elementy jako usunięte
	- $indeks = Search(key)$
	- if $(t[indeks]) != NULL)$ - oznacz $t[indeks]$ jako usunięty

##### Haszowanie zamknięte - SEARCH(key), INSERT(key)
###### SEARCH(key) - próbuj dopóki nie znajdziesz k lub pustego miejsca
- $i=0$
- dopóki $(t[r(i,k)]!=k AND t[r(i,k)]!=NULL)$ - $i=i+1$
- return $r(i,k)$

###### INSERT(key) - próbuj dopóki nie znajdziesz pustego miejsca albo oznaczonego jako usunięty element
- $i=0$
- dopóki $(t[r(i,k))]!=NULL AND t[r(i,k)]!=DEL)$ - $i=i+1$
- $t[r(i,k)]=k$

#### Wypełnienie tablicy
- stopień wypełnienia tablicy haszującej to stosunek liczby przechowywanych elementów do jej rozmiaru
- optymalne wypełnienie dla haszowania zamkniętego, które jest szczególnie wrażliwe na wypełnienie tablicy, to 70%-80%
- zwiększenie stopnia wypełnienia tablicy zwiększa czas wykonywania poszczególnych operacji
- dla haszowania otwartego czas wykonywania operacji rośnie liniowo w stosunku do współczynnika wypełnienia tablicy
- ##### Rozwiązanie:
	- Przy implementacji tablic haszujących ustala się stopień wypełnienia tablicy przy którym należy powiększyć tablicę haszującą

#### Zalety sposobów haszowania

| haszowanie OTWARTE                       | haszowanie ZAMKNIĘTE                                    |
| ---------------------------------------- | ------------------------------------------------------- |
| łatwa implementacja                      | oszczędność miejsca przy małych kluczach                |
| mała wrażliwość na przepełnienie tablicy | możliwość wykorzystania efektu cachu                    |
| brak problemu z usuniętymi elementami    | mniejsza liczba operacji przy małym wypełnieniu tablicy |
|                                          | łatwiejsze zrównoleglenie operacji                      |
