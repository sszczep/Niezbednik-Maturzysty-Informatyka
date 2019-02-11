---
title: Warunek trójkąta
categories: algorytmy
categoryID: algorytmy
---
Warunek istnienia trójkąta jest spełniony, gdy najdłuższy odcinek jest krótszy od sumy dwóch pozostałych. Zakładając że $$ a $$ i $$ b $$ to najkrótsze odcinki, musi zajść nierówność: 

$$ a + b > c $$

Jeżeli nie chcemy wyznaczać najkrótszych odcinków, możemy ułożyć układ trzech nierówności:

$$
\begin{cases}
a + b > c \\ 
a + c > b \\ 
b + c > a
\end{cases}
$$

Powyższy układ jest łatwiejszy do zaimplementowania. Nie musimy przejmować się kolejnością wprowadzanych długości bądź ich sortowaniem.

Funkcja realizująca sprawdzanie warunku istnienia trójkąta w języku C++:
{% highlight cpp linenos %}
bool czyTrojkatIstnieje(double a, double b, double c) {
	return (a + b > c) && 
		   (a + c > b) && 
		   (b + c > a);
}
{% endhighlight %}

<br />

### Klasyfikacja trójkątów ze względu na długości boków

**różnoboczny** - trójkąt o trzech bokach różnej długości

**równoramienny** - trójkąt o dwóch bokach (ramionach) tej samej długości

**równoboczny** - trójkąt o trzech bokach tej samej długości

<br />

### Klasyfikacja trójkątów ze względu na miary kątów

**ostrokątny** - trójkąt o trzech kątach ostrych

**prostokątny** - trójkąt z jednym kątem prostym

**rozwartokątny** - trójkąt z jednym kątem rozwartym

Wychodząc z twierdzenia cosinusów, możemy ustalić rodzaj trójkąta na podstawie długości jego boków:

$$
\begin{cases}
 a^2 + b^2 > c^2 & \text{ trójkąt ostrokątny } \\ 
 a^2 + b^2 = c^2 & \text{ trójkąt prostokątny } \\
 a^2 + b^2 < c^2 & \text{ trójkąt rozwartokątny }
\end{cases}
$$

<br />

### Sprawdzanie rodzaju trójkąta - kod C++

{% highlight cpp linenos %}
#include <iostream>
#include <algorithm>
using namespace std;

enum KlasyfikacjaBoki {
	roznoboczny,
	rownoramienny,
	rownoboczny
};

enum KlasyfikacjaKaty {
	ostrokatny,
	prostokatny,
	rozwartokatny
};

class Trojkat {
	double m_a, m_b, m_c;

	KlasyfikacjaBoki klasyfikujBoki();
	KlasyfikacjaKaty klasyfikujKaty();

	public:
		// Konstruktor przyjmujący długości boków trójkąta
		Trojkat(double a, double b, double c);

		// Metoda wyświetlająca informacje o trójkącie
		void info();
};

Trojkat::Trojkat(double a, double b, double c) : m_a(a), m_b(b), m_c(c) {
	// Sortowanie długości boków rosnąco
	if(m_a > m_b) swap(m_a, m_b);
	if(m_a > m_c) swap(m_a, m_c);
	if(m_b > m_c) swap(m_b, m_c);

	// Wyświetl informacje o trójkącie
	info();
}

KlasyfikacjaBoki Trojkat::klasyfikujBoki() {
	// Jeżeli trójkąt ma wszystkie boki rózne to jest różnoboczny
	if(m_a != m_b && m_a != m_c && m_b != m_c) return roznoboczny;

	// Jeżli trójkąt ma parę takich samych boków (a i b, lub b i c)
	if(m_a == m_b || m_b == m_c) {

		// Jeżeli trójkąt ma rózne boki a i c to jest rownoramienny
		// Wówczas ramiona są parą boków a i b lub b i c
		if(m_a != m_c) return rownoramienny;

		// W przeciwnym wypadku jest równoboczny (a == b == c)
		else return rownoboczny;
	}
}

KlasyfikacjaKaty Trojkat::klasyfikujKaty() {
	const double sumaKwadratow = m_a * m_a + m_b * m_b;
	const double kwadratC = m_c * m_c;

	// Zależności wynikające z twierdzenia cosinusów:

	// Jeżeli suma kwadratów a i b jest większa od kwadratu c to jest ostrokątny
	if(sumaKwadratow > kwadratC) return ostrokatny;

	// Jeżeli suma kwadratów a i b jest równa kwadratowi c to jest prostokątny
	if(sumaKwadratow == kwadratC) return prostokatny;

	// W przeciwnym wypadku jest rozwartokątny
	else return rozwartokatny;
}

void Trojkat::info() {
	cout << "Trójkąt o bokach "
		 << m_a << ", "
		 << m_b << ", "
		 << m_c << " ";

	// Jeżeli trójkąt nie istnieje, wypisz odpowiedni tekst
	if(m_a + m_b <= m_c) {
		cout << "nie istnieje" << endl;
		return;
	}

	// W przeciwnym wypadku określ rodzaj trójkąta

	// W zależności od zwróconej wartości przez metody klasyfikuj*
	// wyświetl odpowiedni tekst
	switch(klasyfikujBoki()) {
		case roznoboczny:
			cout << "różnoboczny";
			break;
		case rownoramienny:
			cout << "równoramienny";
			break;
		case rownoboczny:
			cout << "równoboczny";
			break;
	}

	cout << " i ";

	switch(klasyfikujKaty()) {
		case ostrokatny:
			cout << "ostrokątny";
			break;
		case prostokatny:
			cout << "prostokątny";
			break;
		case rozwartokatny:
			cout << "rozwartokątny";
			break;
	}

	cout << endl;
}

int main() {
	// Sprawdzamy poprawność algorytmu dla różnych trójkątów
	Trojkat(7, 3.5, 3.5);
	Trojkat(4, 5, 6);
	Trojkat(5, 3, 4);
	Trojkat(12, 8, 5);
	Trojkat(12, 16, 12);
	Trojkat(4, 4, 6);
	Trojkat(3, 3, 3);
}
{% endhighlight %}

#### Wyjście programu

{% highlight none %}
Trójkąt o bokach 3.5, 3.5, 7 nie istnieje
Trójkąt o bokach 4, 5, 6 jest różnoboczny i ostrokątny
Trójkąt o bokach 3, 4, 5 jest różnoboczny i prostokątny
Trójkąt o bokach 5, 8, 12 jest różnoboczny i rozwartokątny
Trójkąt o bokach 12, 12, 16 jest równoramienny i ostrokątny
Trójkąt o bokach 4, 4, 6 jest równoramienny i rozwartokątny
Trójkąt o bokach 3, 3, 3 jest równoboczny i ostrokątny
{% endhighlight %}