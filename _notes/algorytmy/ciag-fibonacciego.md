---
title: Ciąg Fibonacciego
categories: algorytmy
categoryID: algorytmy
---
Ciąg Fibonaciego to ciąg liczb naturalnych określonych **rekurencyjnie**, gdzie kolejny wyraz ciągu jest wyrażony przez sumę poprzednich dwóch wyrazów. 

<br />

$$
F_n = \begin{cases}
0 & \text{ dla } n=0 \\ 
1 & \text{ dla } n=1 \\ 
F_{n-2} + F_{n-1} & \text{ dla } n>1
\end{cases}
$$

<br />

$$
\begin{array}{ c }
 F_0 & F_1 & F_2 & F_3 & F_4 & F_5 & F_6 & F_7 & F_8 & F_9 & F_n \\ 
 \hline
 0 & 1 & 1 & 2 & 3 & 5 & 8 & 13 & 21 & 34 & F_{n-2} + F_{n-1}
\end{array}
$$

<br />

## Wariacje ciągu

Istnieją też ciągi wyższych stopni, na przykład: ciąg Tribonacciego, Tetranacciego, Pentanacciego, Hexanacciego itd. Różnią się jedynie ilością sumowanych poprzednich wyrazów ciągu. 

<br />

### Ciąg Tribonacciego

Sumujemy trzy poprzednie wyrazy ciągu. 

<br />

$$
F_n = \begin{cases}
0 & \text{ dla } n<2 \\ 
1 & \text{ dla } n=2 \\ 
F_{n-3} + F_{n-2} + F_{n-1} & \text{ dla } n>2
\end{cases}
$$

<br />

### Ciąg Tetranacciego

Sumujemy cztery poprzednie wyrazy ciągu. 

<br />

$$
F_n = \begin{cases}
0 & \text{ dla } n<3 \\ 
1 & \text{ dla } n=3 \\ 
F_{n-4} + F_{n-3} + F_{n-2} + F_{n-1} & \text{ dla } n>3
\end{cases}
$$

<br />

### Wzór ogólny

Dowolny ciąg $$ k $$ stopnia możemy wyrazić następująco:

<br />

$$
F_n = \begin{cases}
0 & \text{ dla } n<(k-1) \\ 
1 & \text{ dla } n=k-1 \\ 
F_{n-k} + \ ... \ + F_{n-1} & \text{ dla } n>k+1
\end{cases}
$$

<br />

## Kod C++
{% highlight cpp linenos %}
#include <iostream>
using namespace std;

int fibonacci(int n) {
	if(n == 0) return 0;
	if(n == 1) return 1;
	
	return fibonacci(n - 2) + fibonacci(n - 1);
}

int tribonacci(int n) {
	if(n == 0 || n == 1) return 0;
	if(n == 2) return 1;
	
	return tribonacci(n - 3) + tribonacci(n - 2) + tribonacci(n - 1);
}

int ogolny(int n, int k = 2) {
	if(n < k - 1) return 0;
	if(n == k - 1) return 1;
	
	int suma = 0;
	for(int i = k; i > 0; i--) {
		suma += ogolny(n - i, k);
	}
	
	return suma;
}

int main() {
	// Dla testu liczymy dziesiąte wyrazy ciągów
	cout << "fibonacci(10) = " << fibonacci(10) << endl; // Ciąg Fibonacciego
	cout << "tribonacci(10) = " << tribonacci(10) << endl; // Ciąg Tribonacciego
	cout << "ogolny(10, 2) = " << ogolny(10, 2) << endl; // Ciąg Fibonacciego (wzór ogólny)
	cout << "ogolny(10, 3) = " << ogolny(10, 3) << endl; // Ciąg Tribonacciego (wzór ogólny)
	cout << "ogolny(10, 4) = " << ogolny(10, 4) << endl; // Ciąg Tetranacciego (wzór ogólny)
}
{% endhighlight %}

### Wyjście programu

{% highlight none %}
fibonacci(10) = 55
tribonacci(10) = 81
ogolny(10, 2) = 55
ogolny(10, 3) = 81
ogolny(10, 4) = 56
{% endhighlight %}

