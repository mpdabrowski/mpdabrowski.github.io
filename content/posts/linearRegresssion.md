+++
date = '2025-04-18T04:14:17+01:00'
draft = false
title = 'Regresja liniowa cz.I'
+++
{{< mathjax >}}
### Wprowadzenie
Regresja liniowa należy do najprostszych i jednocześnie wciąż szeroko stosowanych metod analizy danych i uczenia maszynowego. Ideą tego algorytmu jest znalezienie liniowej zależoności pomiędzy parametrami $x$ oraz oczekiwaną wartością zmiennej $y$. Liniowej to znaczy takiej, którą da się wyrazić za pomocą wielomianu pierwszego stopnia. Po znalezieniu odpowiedniego wielomianu będziemy w stanie dokonywać wiarygodnych predykcji dla nowych danych. Szukany przez nas wielomian będzie miał postać:
$$
    f_{ \overrightarrow{a},b}(\overrightarrow{x}) = a_1x_1 + a_2x_2 + ... +a_nx_n + b 
$$
gdzie:\
$f_{ \overrightarrow{a},b}(\overrightarrow{x})$ odpowiada $y$ wyrażonemu jako funkcja $x$, \
$x_j$ oznacza kolejne parametry danych wejściowych (odpowiadające jednemu wierszowi w zbiorze danych),\
$a_j$ oraz $b$ to szukane parametry równania.\
Celem budowy modelu regresji liniowej jest znalezienie takich wartości parametrów $a$ i $b$, które w najlepszy sposób odpowiadają analizowanym danym.
![alt](/images/reg_lin1.png)
*Rys. 1. Prosty przypadek regresji liniowej dla jednej cechy.*\
Rysunek 1 przedstawia przykład regresji liniowej dla danych składających się z jednej cehcy $x$. Oś pozioma zawiera wartości $x$, zaś oś pionowa odpowiadające im wartości $y$. Widoczna na rysunku niebieska linia jest wykresem funkcji liniowej otrzymanej dzięki regresji liniowej.
### Funkcja kosztu
W jaki sposób sprawdzić czy wygenerowana przez model funkcja odpowiada analizowanym danym? Do tego celu należy użyć funkcji kosztu. Zadaniem tej funkcji jest zmierzenie odległości pomiędzy rzeczywistymi wartościami $y$, a wartościami zwracanymi przez przygotowany model, które możemy oznaczyć jako $\widehat{y}$. Na rysunku 2 za pomocą zielonych strzałek przedstawione zostały różnice między wartościami rzeczywistymi y oraz wartościami przewidywanymi przez regresję liniową. Czym krótsza strzałka tym przewidywane wartości są bliższe tym prawdziwym.
![alt](/images/reg_lin2.png)
*Rys. 2. Regresja liniowa dla jednej cechy z naniesionymi różnicami między rzeczywistymi oraz przewidywanymi przez regresję wartościami.*\
Przykładem funkcji kosztu, na którym skupimy się w tym artykule, jest błąd średniokwadratowy (ang. mean squar error). Oto jego wzór:
$$ 
J(\overrightarrow{a},b) = \frac{1}{m}\sum_{i=1}^m (f_{a,b}(x^{(i)}) - y^{(i)})^2.
$$
### Metoda gradientu prostego
Metoda gradientu prostego jest metodą optymalizacji (minimalizacji) funkcji wypukłych. W uczeniu maszynowym znaleźć możemy wiele problemów optymalizacyjnych tj. takich w który konieczne jest znalezienie ekstremów funkcji. W kontekście problemu regresji liniowej, wykorzystamy metodę gradientu prostego do minimalizacji funkcji kosztu. Naszym celem jest znalezienie takich wartości parametrów $a_i$ i $b$ dla których wartość błędu średniokwadratowego $J(\overrightarrow{a},b)$ będzie jak najmniejsza.\
 Gradient jest pojęciem z zakresu rachunku rózniczkowego oraz analizy wektorowej. W przypadku funkcji skalarnej wielu zmiennych $f(x_1, x_2, ...)$ gradient jest wektorem zawierającym wszystkie pochodne cząstkowe tej funkcji:
$$
\nabla f= \begin{pmatrix}
\frac{\partial f}{\partial x_1}, \frac{\partial f}{\partial x_2}, \frac{\partial f}{\partial x_3}, ...
\end{pmatrix}.
$$
To co istotne w kontekście minimalizacji funkcji kosztu to to, że gradient niesie w sobie informację o kierunku oraz szybkości z jaką zmienia się funkcja w danym punkcie, a metoda gradientu prostego wykorzystuje je do poszukiwania minimuw. Algorytm metody gradientu powtarza dwie operację
$$
        a_j = a_j - \alpha \frac{\partial}{\partial a_j} J(\overrightarrow{a},b), j = 1,...,n
$$

oraz 

$$
        b = b - \alpha \frac{\partial}{\partial b} J(\overrightarrow{a},b)
$$

do momentu znalezienia minimum. Współczynnik $\alpha$ znajdujący się w powyższych wzorach nazywamy tempem uczenia (learning rate). Określa on krok który algorytm wykonuje w każdej iteracji w kierunku minimalizacji funkcji kosztu.
### Uproszczone przypadki i intuicje
W celu lepszego zrozumienia działania metody gradientu prostego, w pierwszej kolejności rozważymy jej działanie dla pewnych uproszczonych przypadków, a następnie ekstrapolujemy jej działanie dla ogólnego przypadku regresji liniowej.\
Rozważmy przypadek w którym badane dane składają się z tylko jednej cechy $x$ na podstawie której przewidujemy wartość $y$. Przykładowo ilość pokoi w mieszkaniu, na podstawie której staramy się przewidzieć jego wartość. Dodatkowo załóżmy, że szukana przez nas funkcja liniowa ma wspołczynnik $b=0$, zatem:
$$
    f(x) = ax.
$$
W takim (uproszczonym do granic przyzwoitości) przypadku błąd średniokwadratowy jest funkcją wyłącznie jednej zmiennej -- $J(a)$. Metoda gradientu prostego w kolejnych iteracjach będzie aktualizowala wyłącznie wartość $a$:
$$
a = a - \alpha \frac{\partial}{\partial a} J(a).
$$
![alt](/images/reg_lin3.png)
*Rys. 3. Wykres funkcji J(a).*\
Rysunek 3 zawiera wykres funkcji $J(a)$. Jak widać funkcja $J(a)$, która reprezentuje wielkość błędu średniokwadratowego, osiąga swoje minimum dla pewnej wartości $a$ Celem metody gradientu prostego jest znalezienie właśnie tej wartości $a$. W każdej iteracji algorytmu obliczana jest wartość $\frac{\partial}{\partial a} J(a)$. Pochodna funkcji w punkcie oznacza nachylenie prostej stycznej do niej w tym punkcie. Zauważmy, że dla funkcji malejącej, takiej jak ta po lewej stronie wykresu z rysunku 3, obliczona wartość pochodnej będzie **ujemna**, tym samym nowa wartość $a$, uzyskana w tej iteracji będzie **większa** od poprzedniej. Analogiczne rozumowanie możemy przeprowadzić dla prawej strony funkcji widocznej na rysunku 3. Jeżeli funkcja jest w danym punkcie rosnąca to styczna do niej prosta również, co oznacza, że jej pochodna jest **dodatnia**, a nowa wartość $a$, uzyskana w tej iteracji jest **mniejsza**. Kroki te są powtarzane aż do momentu znalezienia minimum funkcji (lub wartości zadowalająco bliskiej wartości minimalnej). Czerwone strzałki widoczne na rysunku 3 reprezentują zmiany wartości $a$ w kolejnych iteracjach. Warto w tym miejscu wspomnieć o współczynniku tempa uczenia $\alpha$. Dobór jego wartości jest niezwykle istotny dla prawidłowego i optymalnego działania algorytmu. W przypadku wyboru zbyt dużego $\alpha$ może dojść do sytuacji w której algorytm w ogóle nie znajdzie minimum, gdyż zmiany wartości $a$ będą zbyt duże w kolejnych iteracjach i będą konsekwentnie omijać wartość minimalną. Z kolei w przypadku wyboru zbyt małej wartości $\alpha$, co prawda algorytm najprawdopodobniej nie będzie miał problemu ze znalezieniem minimum, ale potrzebna do tego ilość iteracji będzie bardzo duża, przez co znacznie wydłuży się również czas działania algorytmu. Oba omówione wcześniej przypadki są przedstawione na rysunku 4.
![alt](/images/reg_lin4.png)
*Rys. 4. Działanie metody gradientu prostego ze zbyt dużym i zbyt małym tempem uczenia.*\
Rozważmy jeszcze jeden, uproszczony przypadek działania metody gradientu prostego. Tym razem funkcja $f(x)$ będzie miała dobrze znaną ze szkoły podstawowej postać:
$$
f(x) = ax + b
$$
Takie sformułowanie problemu nie tylko zbliża nas do rozwiązania ogólnego, ale pozwala również wprowadzić pewną nową, ułatwiającą zrozumienie działania algorytmu ilustrację.
![alt](/images/reg_lin5.png)
*Rys. 5. Wykres przedstawiający zmiany współczynników $a$ i $b$ podczas działania algorytmu.*\
Na rysunku 5 przedstawiony został wykres ilustrujący działanie metody gradientu prostego dla dwóch współczynników. Możemy go rozumiemieć jako spojrzenie z góry na wykresy widoczne na rysunkach 3 i 4. Czerwony krzyżyk widoczny na środku wykresu w najmniejszej elipsie oznacza minimum funkcji. Wykres przedstawia to jak zmiany współczynników $a$ i $b$ obliczane jako:
$$
        a = a - \alpha \frac{\partial}{\partial a} J(a,b) 
$$

oraz 

$$
        b = b - \alpha \frac{\partial}{\partial b} J(a,b)
$$ 
wpływają na poszukiwanie tego minimum.
### Podsumowanie
Mam nadzieję, że przedstawione w poprzednim podrozdziale intuicje pomogły nam zrozumieć istotę działania metody gradientu prostego. Drugi omówiony przypadek jest już bardzo podobny do przypadku ogólnego który wcześniej wyraziliśmy za pomocą wzorów:
$$
        a_j = a_j - \alpha \frac{\partial}{\partial a_j} J(\overrightarrow{a},b) 
$$

oraz 

$$
        b = b - \alpha \frac{\partial}{\partial b} J(\overrightarrow{a},b)
$$
gdzie $j = 1, ..., n$. Chociaż działanie przypadku ogólnego trudniej zilustrować za pomocą prostych wykresów, to wszystko to czego dowiedzieliśmy się za pomocą dwóch, omówionych, przykładów ma przełożenie na problem z większą liczbą współczynników. Metoda gradientu prostego, minimalizując funkcję kosztów pozwala nam na znalezienie najlepiej dobranych współczynników dla liniowej funkcji, którą w regresji liniowej staramy się dopasować do danych. W kolejnej części tego artykułu przedstawię implementację tej metody oraz jej działanie na prawdziwym zestawie danych.
