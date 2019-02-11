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

**róźnoboczny** - trójkąt o trzech bokach różnej długości

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
#include <cmath>
#include <algorithm>
#include <array>
using namespace std;

typedef array<double, 3> DlugosciBokow;

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

bool czyIstnieje(DlugosciBokow& boki) {
	return boki[0] + boki[1] > boki[2];
}

KlasyfikacjaBoki klasyfikujBoki(DlugosciBokow& boki) {
	if(boki[0] != boki[1] && boki[1] != boki[2]) return roznoboczny;
	if(boki[0] == boki[1] || boki[1] == boki[2]) {
		if(boki[0] != boki[2]) return rownoramienny;
		else return rownoboczny;
	}
}

KlasyfikacjaKaty klasyfikujKaty(DlugosciBokow& boki) {
	const double sumaKwadratow = pow(boki[0], 2) + pow(boki[1], 2);
	const double kwadratC = pow(boki[2], 2);
	
	if(sumaKwadratow > kwadratC) return ostrokatny;
	if(sumaKwadratow == kwadratC) return prostokatny;
	else return rozwartokatny;
}

void sprawdzTrojkat(DlugosciBokow& boki) {
	sort(boki.begin(), boki.end());
	
	if(!czyIstnieje(boki)) {
		cout << "Trójkąt o bokach " 
			 << boki[0] << ", " 
			 << boki[1] << ", " 
			 << boki[2] << " nie istnieje" << endl;
			 
		return;
	}
	
	KlasyfikacjaBoki rodzajBoki = klasyfikujBoki(boki);
	KlasyfikacjaKaty rodzajKaty = klasyfikujKaty(boki);
	
	cout << "Trójkąt o bokach " 
		 << boki[0] << ", " 
		 << boki[1] << ", " 
		 << boki[2] << " jest ";
		 
	switch(rodzajBoki) {
		case roznoboczny:
			cout << "różnoboczny";
			break;
		case rownoramienny:
			cout << "równoramienny";
			break;
		case rownoboczny:
			cout << "równoboczny";
	}
	
	cout << " i ";
	
	switch(rodzajKaty) {
		case ostrokatny:
			cout << "ostrokątny";
			break;
		case prostokatny:
			cout << "prostokątny";
			break;
		case rozwartokatny:
			cout << "rozwartokątny";
	}
	
	cout << endl;
}

int main() {
	// Sprawdzamy kilka rodzajów trójkątów
	DlugosciBokow trojkat1 = { 7, 3.5, 3.5 };
	DlugosciBokow trojkat2 = { 4, 5, 6 };
	DlugosciBokow trojkat3 = { 5, 3, 4 };
	DlugosciBokow trojkat4 = { 12, 8, 5 };
	DlugosciBokow trojkat5 = { 12, 16, 12 };
	DlugosciBokow trojkat6 = { 4, 4, 6 };
	DlugosciBokow trojkat7 = { 3, 3, 3 };
	
	sprawdzTrojkat(trojkat1);
	sprawdzTrojkat(trojkat2);
	sprawdzTrojkat(trojkat3);
	sprawdzTrojkat(trojkat4);
	sprawdzTrojkat(trojkat5);
	sprawdzTrojkat(trojkat6);
	sprawdzTrojkat(trojkat7);
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