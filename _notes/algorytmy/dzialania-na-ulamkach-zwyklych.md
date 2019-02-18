---
title: Działania na ułamkach zwykłych
categories: algorytmy
categoryID: algorytmy
---
Przy dodawaniu, odejmowaniu i skracaniu ułamków niezbędna będzie wiedza o wyznaczaniu NWD i NWW. Polecam przeczytać najpierw <a href="{{'/algorytmy/algorytm-euklidesa-wyznaczanie-nwd-i-nww' | relative_url}}" target="_blank">tę</a> notatkę.

### Dodawanie i odejmowanie ułamków

$$ \tfrac{a}{b} \pm \tfrac{c}{d} = \tfrac{ad \pm bc}{bd} $$

Rozwiązanie to ma jedną znaczącą wadę - wychodzą bardzo duże liczby. Weźmy dla przykładu $$ \tfrac{70}{128} + \tfrac{30}{64} $$:

$$
\tfrac{70}{128} + \tfrac{30}{64} =
\tfrac{70*64 + 30*128}{128*64} =
\tfrac{4480 + 3840}{8192} =
\tfrac{8320}{8192} =
\tfrac{65}{64}
$$

Przy tak małych liczbach róznica nie jest jeszcze aż tak widoczna, ale nietrudno przekroczyć zakres inta przy większych ułamkach.

Najłatwiej by było rozszerzyć oba ułamki do najmniejszego wspólnego mianownika:

$$
\tfrac{70}{128} + \tfrac{30}{64} =
\tfrac{70}{128} + \tfrac{60}{128} =
\tfrac{70+60}{128} =
\tfrac{130}{128} =
\tfrac{65}{64}
$$

Jak znaleźć wspólny mianownik? Posłużymy się wcześniej poznanym NWW. Wspólny mianownik będzie równy najmniejszej wspólnej wielokrotności obu mianowników, czyli:

$$ mianownik_{nowy} = NWW(mianownik_{stary1}, mianownik_{stary2}) $$

Następnie musimy wyznaczyć nową wartość liczników. Dla każdego ułamka należy obliczyć ile razy zwiększył się jego mianownik, a następnie otrzymaną wielkość pomnożyć przez stary licznik:

$$ licznik_{nowy} = licznik_{stary} * \tfrac{mianownik_{nowy}}{mianownik_{stary}} $$

Wyznaczmy wzór ogólny:

$$
\tfrac{a}{b} \pm \tfrac{c}{d} =
\tfrac{a * \tfrac{NWW(b, d)}{b}}{NWW(b, d)} \pm \tfrac{c * \tfrac{NWW(b, d)}{d}}{NWW(b, d)}
$$

Dodajmy teraz wcześniejszą parę ułamków stosując powyższe równanie:

$$ NWW(b, d) = NWW(128, 64) = 128 $$

$$
\tfrac{70}{128} + \tfrac{30}{64} =
\tfrac{70 * \tfrac{128}{128}}{128} + \tfrac{30 * \tfrac{128}{64}}{128} =
\tfrac{70 * 1}{128} + \tfrac{30 * 2}{128} =
\tfrac{70}{128} + \tfrac{60}{128} =
\tfrac{70 + 60}{128} =
\tfrac{130}{128}
$$

Skoro dodawanie i odejmowanie mamy za sobą, przejdźmy do mnożenia i dzielenia.

<br />

### Mnożenie i dzielenie ułamków

Tutaj, jako że nie musimy sprowadzać ułamków do tego samego mianownika, będzie łatwiej.

Przy mnożeniu ułamków, mnożymy wszystkie ich liczniki i mianowniki:

$$
\tfrac{a}{b} * \tfrac{c}{d} =
\tfrac{ac}{bd}
$$

Przy dzieleniu ułamków, mnożymy pierwszy ułamek przez odwrotność drugiego. Dzięki temu mamy pewność, że wynik będzie typu całkowitego (mnożymy dwie liczby całkowite).

$$
\tfrac{\tfrac{a}{b}}{\tfrac{c}{d}} =
\tfrac{a}{b} * \tfrac{d}{c} =
\tfrac{ad}{bc}
$$

<small>Możemy jeszcze skracać ułamki podczas ich mnożenia, ale to zostawiam już dla was :). Podpowiem że przydatne będzie $$ NWD(a, d) $$ i $$ NWD(b, c) $$. Nie powinniście mieć problemu z zaimplementowaniem tego po następnym podpunkcie.</small>

<br />

Jako że wiemy już jak wykonywać podstawowe działania na ułamkach, pora na skracanie ich do najprostszej postaci.

<br />

### Skracanie ułamków

Aby skrócić ułamek do najprostszej postaci, należy wyznaczyć największy wspólny dzielnik jego licznika i mianownika i skrócić go przez tą liczbę:

$$ 
\tfrac{a}{b} =
\tfrac{\tfrac{a}{NWD(a, b)}}{\tfrac{b}{NWD(a, b)}}
$$

Skróćmy ułamek z poprzedniego przykładu:

$$ NWD(130, 128) = 2 $$

$$
\tfrac{130}{128} =
\tfrac{\tfrac{130}{2}}{\tfrac{128}{2}} =
\tfrac{65}{64}
$$

To by było na tyle z teorii. Pora na kod realizujący powyższe operacje w praktyce.

<br />

### Kod C++

{% highlight cpp linenos %}
#include <iostream>
#include <algorithm>
using namespace std;

// W całym programie zakładam, że mianownik nigdy nie będzie równy 0
// Dzięki temu nie musimy tego sprawdzać przy wyliczaniu NWW

// Po wytłumaczenie jak wyliczać NWD i NWW odsyłam do notatki o Algorytmie Euklidesa
// https://sszczep.github.io/Niezbednik-Maturzysty-Informatyka/algorytmy/algorytm-euklidesa-wyznaczanie-nwd-i-nww

int NWD(int a, int b) {
	while(b) swap(a %= b, b);
	
	return a;
}

int NWW(int a, int b) {
	return a * b / NWD(a, b);
}

// Struktura przechowywująca licznik i mianownik oraz metody dotyczące działań na ułamkach
struct Ulamek {
	int a, b;
	
	void dodaj(Ulamek ulamek) {
		// Jeżeli ułamki mają te same mianowniki, wystarczy dodać liczniki
		if(b == ulamek.b) {
			a += ulamek.a;
			return;
		}
		
		// W przeciwnym wypadku liczymy wspólny mianownik i nowe liczniki
		int mianownik = NWW(b, ulamek.b);
		int licznik1 = a * (mianownik / b);
		int licznik2 = ulamek.a * (mianownik / ulamek.b);
		
		a = licznik1 + licznik2;
		b = mianownik;
	}
	
	void odejmij(Ulamek ulamek) {
		// Zmieniamy znak licznika na przeciwny i dodajemy ułamki
		ulamek.a *= -1;
		dodaj(ulamek);
	}
	
	void pomnoz(Ulamek ulamek) {
		// Mnożymy liczniki i mianowniki
		a *= ulamek.a;
		b *= ulamek.b;
	}
	
	void podziel(Ulamek ulamek) {
		// Odwracamy drugi ułamek i mnożymy
		swap(ulamek.a, ulamek.b);
		pomnoz(ulamek);
	}
	
	void skroc() {
		// Wyznaczamy NWD i skracamy ulamek
		int nwd = NWD(a, b);
		a /= nwd;
		b /= nwd;
	}
	
	void info() {
		cout << a << "/" << b;
	}
};

int main() {
	Ulamek ulamek{70, 128};
	cout << "Początkowy ułamek: ";
	ulamek.info();
	
	ulamek.dodaj(Ulamek{30, 64});
	cout << "\n70/128 + 30/64 = ";
	ulamek.info();
	
	ulamek.skroc();
	cout << "\nSkrócony: ";
	ulamek.info();
	
	ulamek.odejmij(Ulamek{17, 64});
	cout << "\n65/64 - 17/64 = ";
	ulamek.info();
	
	ulamek.skroc();
	cout << "\nSkrócony: ";
	ulamek.info();
	
	ulamek.pomnoz(Ulamek{2, 4});
	cout << "\n3/4 * 2/4 = ";
	ulamek.info();
	
	ulamek.skroc();
	cout << "\nSkrócony: ";
	ulamek.info();
	
	ulamek.podziel(Ulamek{3, 8});
	cout << "\n3/8 / 3/8 = ";
	ulamek.info();
	
	ulamek.skroc();
	cout << "\nSkrócony: ";
	ulamek.info();
}
{% endhighlight %}

#### Wyjście programu

{% highlight none %}
Początkowy ułamek: 70/128
70/128 + 30/64 = 130/128
Skrócony: 65/64
65/64 - 17/64 = 48/64
Skrócony: 3/4
3/4 * 2/4 = 6/16
Skrócony: 3/8
3/8 / 3/8 = 24/24
Skrócony: 1/1
{% endhighlight %}