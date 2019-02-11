---
title: Liczby pierwsze - sito Erastotenesa
categories: algorytmy
categoryID: algorytmy
---
Liczby pierwsze to takie liczby naturalne większe od 1, które mają dwa dzielniki naturalne - 1 i siebie samą. Liczby większe od 1 niebędące liczbami pierwszymi nazywami liczbami złożonymi.

Przykładowe liczby pierwsze: $$ 2, 3, 5, 7, 11, 13, 17 $$

<br />

### Liczby pierwsze bliźniacze
Parę liczb pierwszych *p* i *q*, nazywamy bliźniaczymi, gdy zachodzi równość $$ p = q + 2 $$.

Przykładem takkich liczb mogą być pary: $$ \{3, 5\}, \{5, 7\}, \{11, 13\} $$.

<br />

### Sprawdzanie czy liczba jest liczbą pierwszą
Aby sprawdzić czy liczba $$ n $$ jest pierwsza, należy przeiterować przez kolejne liczby z przedziału $$ \langle 2, \sqrt{n}\rangle $$.

Przykładowa funkcja w języku C++:

{% highlight cpp linenos %}
bool czyPierwsza(int liczba) {
	// sprawdzamy wszystkie liczby naturalne z przedzialu <2, sqrt(n)>
	for(int i = 2; i <= sqrt(liczba); i++) {
		// jeżeli znaleźliśmy dzielnik,
		// liczba nie jest pierwsza,
		// zwracamy fałsz
		if(liczba % i == 0) return false;
	}

	// jeżeli nie znaleźliśmy żadnego dzielnika,
	// oznacza to że liczba jest pierwsza,
	// zwracamy prawdę
	return true;
}
{% endhighlight %}

Przykładowe wywołanie funkcji:
{% highlight cpp linenos %}
const bool pierwsza = czyPierwsza(n);
std::cout << "Liczba " << (pierwsza ? "" : "nie ") << "jest pierwsza";
{% endhighlight %}

Liczba n | Dzielniki n | Dane wyjściowe programu
--- | --- | ---
11 | 1, 11 | Liczba jest pierwsza
15 | 1, 3, 5, 15 | Liczba nie jest pierwsza

<br />

### Sito Erastotenesa

Sito Erastotenesa pozwala nam na szybkie wyznaczanie liczb pierwszych z przedzialu $$ \langle 2, n \rangle $$. Strategia nie jest skomplikowana. Wystarczy znajdować kolejne liczby pierwsze i wykreślać jej wielokrotności. Dla przykładu, zaprezentuję przesiewanie liczb od 2 do 16. Na początku tworzymy tablicę wszystkich liczb:

$$
\require{cancel}
\begin{Bmatrix}
 \cancel{1} & 2 & 3 & 4 \\ 
 5 & 6 & 7 & 8 \\ 
 9 & 10 & 11 & 12 \\ 
 13 & 14 & 15 & 16 
\end{Bmatrix}
$$

Zaczynamy od wszystkich wielokrotności 2:

$$
\require{cancel}
\begin{Bmatrix}
 \cancel{1} & 2 & 3 & \cancel{4} \\ 
 5 & \cancel{6} & 7 & \cancel{8} \\ 
 9 & \cancel{10} & 11 & \cancel{12} \\ 
 13 & \cancel{14} & 15 & \cancel{16} 
\end{Bmatrix}
$$

To samo robimy dla kolejnej liczby czyli 3:

$$
\require{cancel}
\begin{Bmatrix}
 \cancel{1} & \cancel{2} & 3 & \cancel{4} \\ 
 5 & \cancel{6} & 7 & \cancel{8} \\ 
 \cancel{9} & \cancel{10} & 11 & \cancel{12} \\ 
 13 & \cancel{14} & \cancel{15} & \cancel{16} 
\end{Bmatrix}
$$

Po sprawdzeniu pozostałych liczb okazuje się, że wykreśliliśmy już ich wszystkie wielokrotności. Jedyne niewykreślone liczby jakie nam zostały to $$ 3, 5, 7, 11, 13 $$, czyli liczby pierwsze z zadanego przedziału. Warto napomnieć, że obowiązuje tutaj ta sama zasada co w sprawdzaniu czy liczba jest pierwsza - wystarczy sprawdzać liczby do $$ \sqrt{n} $$.

Implementacja algorytmu w języku C++:
{% highlight cpp linenos %}
#include <iostream>
#include <cmath>
using namespace std;

void sito(bool tablica[], int n) {
	// tablica domyślnie jest wypelniona wartościami false
	// dla wszystkich liczb >= 2, ustawiamy wartość początkową true
	for(int i = 2; i <= n; i++) {
		tablica[i] = true;
	}
	
	for(int i = 2; i <= sqrt(n); i++) {
		// jeżeli liczba jest już wykreślona,
		// nie ma potrzebny sprawdzać jej wielokrotności
		// (również będą już wykreślone)
		if(tablica[i] == false) continue;
		
		// wykreślamy wielokrotności liczby
		for(int j = i * 2; j <= n; j += i) {
			tablica[j] = false;
		}
	}
}

int main() {
	const int n = 100;
	
	// inicjalizujemy tablicę o wielkości n + 1
	// dla uproszczenia algorytmu, tablica będzie mieścić liczby od 0 do n
	// dzięki temu indeks będzie reprezentować liczbę
	bool tablica[n + 1] = {};
	
	// wywołujemy funkcję przesiewającą tablicę
	sito(tablica, n);
	
	// wyświetlamy wszystkie liczby pierwsze
	for(int i = 2; i <= n; i++) {
		if(tablica[i]) cout << i << " ";
	}
}
{% endhighlight %}