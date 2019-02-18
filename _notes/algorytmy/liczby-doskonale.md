---
title: Liczby doskonałe
categories: algorytmy
categoryID: algorytmy
---
Liczby doskonałe to takie liczby, których suma dzielników od niej mniejszych jest równa tej liczbie.

Trzy pierwsze liczby doskonałe:

$$
\text{Liczba: }6 \\
D_6 = \{1, 2, 3\} \\
\text{Suma }D_6 = 6
$$

$$
\text{Liczba: }28 \\
D_{28} = \{1, 2, 4, 7, 14\} \\
\text{Suma }D_{28} = 28
$$

$$
\text{Liczba: }496 \\
D_{496} = \{1, 2, 4, 8, 16, 31, 62, 124, 248\} \\
\text{Suma }D_{496} = 496
$$

<br />

### Generowanie liczb doskonałych

Aby wygenerować liczby doskonałe, skorzystamy ze wzoru:

$$ N = 2^{k-1} * (2^k - 1) $$

gdzie $$ k $$ i $$ 2^k - 1 $$ są liczbami pierwszymi.

Przykłady:

$$
\require{color}

\begin{array}{ l }
k & 2^{k-1} & 2^k - 1 & N \\
\hline
2 & 2 & 3 & 2*3=6 \\
\hline
3 & 4 & 7 & 4*7=28 \\
\hline
5 & 16 & 31 & 16*31=496 \\
\hline
7 & 64 & 127 & 64*127=8128 \\
\hline
11 & 1024 & 2047 \notin \mathbb{P} & {\color{red}1024*2047=2096128} \\
\hline
13 & 4096 & 8191 & 4096*8191=33550336
\end{array}
$$

Jak można zauważyć, dla $$ k = 11 $$ nie ma liczby doskonałej (2047 nie jest liczbą pierwszą).

Najlepszym sposobem na generowanie liczb pierwszych byłoby użycie <a href="{{'/algorytmy/liczby-pierwsze-sito-erastotenesa' | relative_url}}" target="_blank">Sita Erastotenesa</a>. Warto mieć jednak na uwadze, że kolejne liczby doskonałe bardzo szybko rosną (piąta liczba doskonała wynosi już 33550336!). Nie będę się zajmował generowaniem liczb doskonałych w algorytmie, raczej nie wystąpi to na maturze.

### Kod C++

{% highlight cpp linenos %}
#include <iostream>
#include <cmath>
using namespace std;

bool czyDoskonala(int liczba) {
	int sumaDzielnikow = 0;

	// Wyznaczamy wszystkie dzielniki liczby i je sumujemy
	for(int i = 1; i < liczba; i++) {
		if(liczba % i == 0) {
			sumaDzielnikow += i;
		}
	}

	// Jeżeli suma dzielników jest równa liczbie, liczba jest doskonałą
	return liczba == sumaDzielnikow;
}

int main() {
	cout << "Sprawdzamy klika liczb, czy są doskonałe" << endl << endl;
	cout << "6 - " << (czyDoskonala(6) ? "doskonała" : "niedoskonała") << endl;
	cout << "7 - " << (czyDoskonala(7) ? "doskonała" : "niedoskonała") << endl;
	cout << "20 - " << (czyDoskonala(20) ? "doskonała" : "niedoskonała") << endl;
	cout << "496 - " << (czyDoskonala(496) ? "doskonała" : "niedoskonała") << endl;
}
{% endhighlight %}

#### Wyjście programu

{% highlight none %}
Sprawdzamy klika liczb, czy są doskonałe

6 - doskonała
7 - niedoskonała
20 - niedoskonała
496 - doskonała
{% endhighlight %}