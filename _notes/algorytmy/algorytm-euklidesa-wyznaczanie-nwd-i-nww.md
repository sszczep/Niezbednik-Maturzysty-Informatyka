---
title: Algorytm Euklidesa - wyznaczanie NWD i NWW
categories: algorytmy
categoryID: algorytmy
---
Największym wspólnym dzielnikiem liczb $$ a $$ i $$ b $$ jest taka liczba $$ c $$, która dzieli $$ a $$ i $$ b $$ przez możliwie jak największą wartość. Największy wspólny dzielnik jest bardzo często używany do skracania ułamków, bądź wyznaczania najmniejszej wspólnej wielokrotności.

<br />

### Algorytm wykorzystujący odejmowanie
Zacznijmy od najprostszego wariantu wyznaczania NWD. Jeżeli od większej liczby zaczniemy odejmować mniejszą i w pewnym momencie będą sobie równe, oznacza to że ich wartości będą szukanym największym wspólnym dzielnikiem. Sprawdźmy to dla pary liczb 105 i 45:

$$
\begin{array}{r|l}
a = 105 & b = 45 \\
\hline
105 - 45 & 45 \\
60 - 45 & 45 \\
15 & 45 - 15 \\
15 & 30 - 15 \\
15 & 15
\end{array}
\\
NWD(105, 45) = 15
$$

Przypadki brzegowe, które należy uwzględnić:

$$ 
NWD(a, 0) = a \\
NWD(0, b) = b \\
NWD(0, 0) = 0
$$

<small>**Uwaga!** $$ NWD(0, 0) = 0 $$ jest sprawą sporną. Na internecie można znaleźć wiele twierdzeń, że jest to wartość niezdefiniowana, nieskończona, czy też równa 0. W algorytmie przyjmuję ją jako równą zero ze względu na uproszczenie całości. [WolframAlpha](https://www.wolframalpha.com/input/?i=GCD(0,0)){:target="_blank"} też podaje że jest równa 0.</small>

Nasz algorytm działa i wydaje się w miarę szybki. Co się jednak stanie, gdy weźmiemy dla przykładu parę liczb 333 i 3?

$$
\begin{array}{r|l}
a = 333 & b = 3 \\
\hline
333 - 3 & 3 \\
330 - 3 & 3 \\
327 - 3 & 3 \\
... & 3\\
6 - 3 & 3 \\
3 & 3 
\end{array}
\\
NWD(333, 3) = 3
$$

Widzicie ile operacji musimy wykonać? Z pomocą przychodzi zoptymalizowany algorytm Euklidesa...

<br />

### Algorytm wykorzystujący metodę modulo
W tym przypadku zamiast odejmowania będziemy wykorzystywać operator modulo czyli resztę z dzielenia. Przyda się nam tutaj pomocnicza zmienna - wartość wyrażenia $$ a \text{ modulo } b $$. $$ a $$ powinno być większe od $$ b $$.

Lista kroków:
1. $$ c = a \text{ modulo } b $$
2. $$ a = b, b = c $$
3. Jeżeli $$ b = 0 $$ to $$ a = NWD(a, b) $$, jeżeli nie to wracamy do pierwszego kroku

Prześledźmy to dla naszych poprzednich par liczb:

$$
\begin{array}{r|l|l}
a = 105 & b = 45 & c = 15 \\
\hline
45 & 15 & 0 \\
15 & 0
\end{array}
\\
NWD(105, 45) = 15
$$

$$
\begin{array}{r|l|l}
a = 333 & b = 3 & c = 0 \\
\hline
3 & 0
\end{array}
\\
NWD(333, 3) = 3
$$

Oj chyba troszkę szybciej ;)  

Co z naszymi przypadkami brzegowymi z poprzedniego algorytmu? Jeżeli $$ b = 0 $$, algorytm poprawnie wskaże $$ NWD(a, 0) = a $$. Nietrudno zauważyć, że jeżeli $$ a = 0 $$, algorytm dalej poda prawidłową odpowiedź czyli $$ 0 $$.

<br />

### NWW - Najmniejsza wspólna wielokrotność

Koniec skomplikowanych rzeczy. Jako że potrafimy już wyliczać NWD, skorzystamy z prostej zależności:

$$ a * b = NWD(a, b) * NWW(a, b) $$

Po przekształceniu otrzymujemy:

$$ NWW(a, b) = \tfrac{a * b}{NWD(a, b)} $$

Przetestujmy powyższą zależność dla naszej pary liczb 105 i 45:

$$ 
NWW(105, 45) = \tfrac{105 * 45}{NWD(105, 45)} \\
NWW(105, 45) = \tfrac{4725}{15} \\
NWW(105, 45) = 315
$$

Zauważmy tylko, że $$ NWD(a, b) $$ nie może być równe 0. Jest to kolejny warunek, o który musimy zadbać. W C++ dzielenie liczby całkowitej przez 0 skutkuje [undefined behavior](https://en.cppreference.com/w/cpp/language/ub){:target="_blank"} co może prowadzić do crasha programu! Jeżeli $$ NWD(a, b) = 0 $$, zwracam $$ NWW(a, b) = 0 $$. Na maturze jednak raczej niepowinny się pojawić takie dane aby NWD i NWW wynosiło 0.

<br />

### Kod C++

{% highlight cpp linenos %}
#include <iostream>
#include <algorithm>
using namespace std;

// Wyznaczanie NWD metodą odejmowania
int odejmowanieNWD(int a, int b) {
	// Obsługujemy przypadki brzegowe
	// Nie musimy obsługiwać osobno NWD(0, 0) = 0
	// ponieważ oba ify zwróciłyby prawdę i wartość 0
	if(b == 0) return a;
	if(a == 0) return b;
	
	// Dopóki liczby są różne, wykonuj pętlę
	while(a != b) {
		// Jeżeli b jest większe od a, zamień ich wartości
		if(b > a) swap(a, b);
		
		// Od a odejmujemy b
		a -= b;
	}
	
	// a = b, więc zwracamy którąkolwiek z wartości
	return a;
}

// Wyznaczanie NWD metodą modulo
int moduloNWD(int a, int b) {
	int c; 
	
	// Dopóki b jest różne od zera, wykonuj pętlę
	while(b != 0) {
		c = a % b;
		a = b;
		b = c;
		
		// Inny sposób zapisania powyższej zawartości pętli:
		// swap(a %= b, b);
		// Dzięki temu zmienna a będzie zawierać wartość zmiennej b,
		// a zmienna b wartość a % b
	}
	
	// b = 0, więc zwracamy a
	return a;
}

int NWW(int a, int b) {
	// Obliczamy wartość NWD(a, b)
	int nwd = moduloNWD(a, b);
	
	// Jeżeli NWD(a, b) = 0, zwracamy 0
	if(nwd == 0) return 0;
	
	// W przeciwnym wypadku liczymy NWW(a, b)
	return a * b / nwd;
}

int main() {
	cout << "odejmowanieNWD(105, 45) = " << odejmowanieNWD(105, 45) << endl;
	cout << "moduloNWD(105, 45) = " << moduloNWD(105, 45) << endl;
	cout << "NWW(105, 45) = " << NWW(105, 45) << endl << endl;

	cout << "odejmowanieNWD(333, 3) = " << odejmowanieNWD(333, 3) << endl;
	cout << "moduloNWD(333, 3) = " << moduloNWD(333, 3) << endl;
	cout << "NWW(333, 3) = " << NWW(333, 3) << endl << endl;

	cout << "odejmowanieNWD(1024, 768) = " << odejmowanieNWD(1024, 768) << endl;
	cout << "moduloNWD(1024, 768) = " << moduloNWD(1024, 768) << endl;
	cout << "NWW(1024, 768) = " << NWW(1024, 768) << endl << endl;

	cout << "Przypadki brzegowe:" << endl << endl;
	
    cout << "odejmowanieNWD(5, 0) = " << odejmowanieNWD(5, 0) << endl;
	cout << "moduloNWD(5, 0) = " << moduloNWD(5, 0) << endl;
	cout << "NWW(5, 0) = " << NWW(5, 0) << endl << endl;

	cout << "odejmowanieNWD(0, 5) = " << odejmowanieNWD(0, 5) << endl;
	cout << "moduloNWD(0, 5) = " << moduloNWD(0, 5) << endl;
	cout << "NWW(0, 5) = " << NWW(0, 5) << endl << endl;

	cout << "odejmowanieNWD(0, 0) = " << odejmowanieNWD(0, 0) << endl;
	cout << "moduloNWD(0, 0) = " << moduloNWD(0, 0) << endl;
	cout << "NWW(0, 0) = " << NWW(0, 0) << endl << endl;
}
{% endhighlight %}

#### Wyjście programu

{% highlight none %}
odejmowanieNWD(105, 45) = 15
moduloNWD(105, 45) = 15
NWW(105, 45) = 315

odejmowanieNWD(333, 3) = 3
moduloNWD(333, 3) = 3
NWW(333, 3) = 333

odejmowanieNWD(1024, 768) = 256
moduloNWD(1024, 768) = 256
NWW(1024, 768) = 3072

Przypadki brzegowe:

odejmowanieNWD(5, 0) = 5
moduloNWD(5, 0) = 5
NWW(5, 0) = 0

odejmowanieNWD(0, 5) = 5
moduloNWD(0, 5) = 5
NWW(0, 5) = 0

odejmowanieNWD(0, 0) = 0
moduloNWD(0, 0) = 0
NWW(0, 0) = 0
{% endhighlight %}