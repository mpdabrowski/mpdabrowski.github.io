+++
date = '2025-01-23T12:28:17+01:00'
draft = false
title = 'Sortowanie topologiczne'
+++
### Wprowadzenie
Sortowanie topologiczne to uporządkowanie wszystkich wierzchołków grafu skierowanego w taki sposób, że każdy wierzchołek znajduje się "przed" wierzchołkami do których prowadzą wychodzące z niego krawędzie. Aby lepiej zrozumieć co to oznacza przyjrzyjmy się poniższemu przykładowi. Rysunek 1 przedstawia przykładowy graf skierowany który chcemy poddać sortowaniu tologicznemu. 

![alt](/images/graf.1.png)
*Rys. 1. Graf skierowany który chcemy poddać sortowaniu topologicznemu.*

Zgodnie z tym co zostało napisane powyżej, chcemy aby po uporządkowaniu każdy wierzchołek znajdował się przed wierzchołkami do których prowadzą wychodzące z niego ścieżki, a zatem **s** powinien znajdować się przed **v** i **w** oraz wszystkie wymienione wierzchołki powinny znajdować się przed **t**. Bazując na tych założeniach możemy zauważyć, że może istnieć więcej niż jedno uporządkowanie topologiczne grafu. Rysunek 2 przedstawia możliwe uporządkowania dla rozważanego przez nas grafu.

![alt](/images/graf.2.png)
*Rys. 2. Dwa możliwe uporządkowania rozważanego grafu.*

### Jakie grafy można posortować topologicznie?
Uporządkowanie topologiczne posiadają wyłącznie acykliczne grafy skierowane, nazywane DAGami (od angielskiego directed acyclic graph - DAG). Cyklem w grafie nazywamy zamkniętą drogę lub ścieżkę. Graf, przedstawiony na rysunku 1, jest przykładem grafu acyklicznego. Możemy zauważyć, że w tym grafie nie istnieje ścieżka która zaczynąłaby się i kończyła w tym samym węźle. Rysunek 3 zawiera ilustrację grafu posiadającego cykl, dla którego niemożliwe byłoby znalezienie uporządkowania topologicznego.

![alt](/images/graf.3.png)
*Rys. 3. Graf zawierający cykl.*

Fakt, iż tego rodzaju grafy nie mogą zostać uporządkowane topologicznie powinien wydać nam się dosyć intuicyjny. W końcu który z wierzchołków wchodzących w skład cyklu znajduje się "przed" innymi? Równie intuicyjną wydaje się być przyczyna tego, czemu nie rozważamy grafów nieskierowanych. Jeżeli krawędzie grafu mogą być przechodzone w obu kierunkach, to ciężko byłoby nam wskazać który wierzchołek ma znajdować się "przed" innymi.

### Definicja
Mając już omówione pojęcie acyklicznego grafu skierowanego możemy przedstawić bardziej formalną definicję sortowania topologicznego.
>**Definicja 1.** Niech *G*=(*V*, *E*) będzie acyklicznym grafem skierowanym. 
Uporządkowanie topologiczne G to przyporządkowanie każdemu 
wierzchołkowi *v* ∈ *V* innej liczby  *f(v)* takiej, że:\
dla każdej krawędzi (*v*, *w*) ∈ *E*, zachodzi *f(v)* < *f(w)*.

Tak sformułowana definicja okaże się użyteczna przede wszystkim przy analizie oraz dowodzeniu poprawności działania algorytmu sortowania topologicznego. Przejdźmy więc do najciekawszej części tego artykułu tj. omówienia algorytmu.

### Algorytm sortowania topologicznego
W tym artykule przedstawiony zostanie algorytm sortowania topologicznego oparty o przeszukiwanie grafu w głąb (ang. depth first search - DFS). DFS jest strategią przeszukiwania grafu w której "sięgamy" tak głęboko w graf jak to tylko możliwe i cofamy się tylko wtedy, kiedy z wierzchołka w którym się znajdujemy nie ma już możliwości sięgnąć głębiej (tj. nie posiada on nieodwiedzonych sąsiadów). Tak też będzie działał nasz algorytm. Będzie on, zaczynając od wierzchołka startowego, sięgał coraz głębiej w graf, a kiedy napotka wierzchołek *v* który nie posiada już nieodwiedzonych sąsiadów, będzie przypisywał mu odpowiednią liczbę *f(v)*. Działanie algorytmu sortowania topologicznego opisuje pseudokod umieszczony poniżej:

```
TOPOSORT(G)
 Input: Acykliczny graf skierowany G=(V,E) w postaci listy sąsiedztwa
 Postcondition: Uporządkowanie topologiczne grafu G.
 mark all v ∈ V as unexplored
 curLabel := |V|
 for v ∈ V do
     if v is unexplored then
    DFS-Topo(G, v)

```
funkcja DFS-Topo wykorzystana w pseudokodzie jest zdefiniowana jako:

```
DFS-Topo(G, s)
 Input: Graf G=(V,E), wierzchołek s.
 Postcondition: Wierzchołki osiągalne z s  oznaczone jako odwiedzone, z przypisanymi wartościami f.
 mark s as explored
 for v in adjacency list of s do
    if v is unexplored then
        DFS-Topo(G, v)
 f(s) := curLabel
 curLabel := curLabel- 1

```
Powyższy algorytm na wejściu przyjmuje listę sąsiectwa. Rysunek 4 przedstawia przykład listy sąsiedztwa dla grafu z rysunku 1.

![alt](/images/graf.4.png)
*Rys. 4. Lista sąsiedztwa grafu.*

W pierwszym kroku algorytm oznacza wszystkie węzły w grafie jako nieodwiedzone. Następnie definiowana jest zmienna ```curLabel``` której wartość ma być równa ilości węzłów w sortowanym grafie. Zmienna ta ma kluczowe znaczenie dla działania algorytmu, ponieważ jest wykorzystywana do przypisywania wierzchołką wartości *f*. W kolejnym kroku, dla każdego wierzchołka grafu wywoływana jest funkcja ```DFS-Topo```. Oznacza ona wierzchołek jako odwiedzony oraz wywołuje się rekurencyjnie dla wszystkich nieodwiedzonych wierzchołków na liście sąsiedztwa pierwotnego wierzchołka. Ostatecznie, kiedy zakończą się wszystkie wywołania dla wierzchołków z listy sąsiedztwa, funkcja przypisuje wierzchołkowi wartość *f* równą aktualniej wartości ```curLabel```, a wartość zmiennej ```curLabel``` jest zmniejszana o 1. To, czemu wartość *f* jest przypisywana wierzchołką w taki właśnie sposób wyjaśnie w dalszej części artykułu, będącej analizą poprawności i złożoności algorytmu. Teraz jednak, w celu lepszego zrozumienia omawianego problemu przyjrzyjmy się implementacji algorytmu napisanej w Pythonie:
```Python
def topological_sort(G):
    def DFS_topo(G, s):
        nonlocal curLabel
        visited[s] = True
        for v in G[s]:
            if False == visited[v]:
                DFS_topo(G, v)
        f[s] = curLabel
        curLabel -= 1
    
    visited = {v: False for v in G}
    curLabel = len(G)
    f = {}
    for v in G:
        if False == visited[v]:
            DFS_topo(G, v)
    
    return sorted(f, key=lambda x: f[x])
```
jak widać na powyższym listingu implementacja sortowania topologicznego w tym akurat języku jest niemal identyczna z pseudokodem.
Jedyne widoczne różnice dotyczą oznaczania i sprawdzania czy wierzchołek został odwiedzony.
Sortowanie widoczne w ostaniej linii mogłoby zostać pominięte i wykonane np. przy wyświetlaniu wyniku działania funkcji.
Nie jest ono bezpośrednio związane z działaniem algorytmu i ma na celu uporządkowanie wierzchołków zgodnie z przypisanymi do nich wartościami *f*.

### Analiza działania algorytmu

W tej części postaram się przedstawić dowód poprawności działania algorytmu oraz jego złożoność obliczeniową.
Zacznijmy od twierdzenia:
>**Twierdzenie 1**: Dla każdego acyklicznego grafu skierowanego *G*=(*V*,*E*) w postaci listy sąsiedztwa:\
>(a) po zakończeniu działania algorytmu ```TOPOSORT```, każdy wierzchołek *v* ma przypisaną wartość *f*, a te wartości *f* stanowią uporządkowanie topologiczne grafu *G*;\
>(b) czas działania algorytmu TOPOSORT wynosi *O(m+n)*, gdzie *m=|E|*, a *n=|V|*.

Powyższe twierdzenia można udowodnić w dosyć prosty sposób:
>**Dowód**: ```DFS-Topo``` zostanie wywołana dla każdego wierzchołka *v* ∈ *V* dokładnie raz, gdy v zostanie po raz pierwszy napotkany. Po zakończeniu wywołania ```DFS-Topo```, wierzchołkowi *v* zostanie przypisana etykieta. Zatem każdy wierzchołek otrzymuje etykietę, a poprzez dekrementację zmiennej ```curLabel``` przy każdym przypisaniu etykiety, algorytm zapewnia, że każdy wierzchołek otrzymuję unikatową etykietę *f(v)* z zakresu *{1, 2, …, |V|}*. Aby przekonać się, dlaczego te etykiety stanowią uporządkowanie topologiczne rozważmy dowolną krawędź *(v, w)*; należy wykazać, że *f(v)* < *f(w)*. Istnieją dwa przypadki, w zależności od tego, który z wierzchołków *v* czy w algorytm odkrywa jako pierwszy.\
**Przypadek 1** *v* jest odkrywany przed *w*:\
Wówczas ```DFS-Topo``` jest wywoływana dla wierzchołka *v* przed tym 
zanim w został oznaczony jako odwiedzony. Ponieważ *w* jest osiągalny z 
*v* (przez krawędź *(v,w)*), to wywołanie ```DFS-Topo``` w końcu odkryje *w* i 
rekurencyjnie wywoła ```DFS-Topo``` dla *w*. Ze względu na fakt, iż 
wywołania rekurencyjne tworzą stos wywołań (struktura last-in first
out), wywołanie ```DFS-Topo``` dla *w* zakończy się przed wywołaniem ```DFS-Topo``` dla *v*. Ponieważ etykiety są przypisywanie w porządku malejącym, 
*w* otrzyma większą wartość niż *v* co jest zgodne z wymogiem 
sortowania topologicznego.\
**Przypadek 2** w jest odkrywane przed *v*:\
 Ponieważ *G* jest grafem skierowanym acyklicznym nie istnieje ścieżka 
prowadząca z *w* do *v*; w przeciwnym razie połączenie takiej ścieżki z 
krawędzią *(v, w)* utworzyłoby graf skierowany. Oznacza to, że 
wywołanie ```DFS-Topo``` rozpoczynające się w w nie może odkryć *v* i kończy 
się gdy *v* jest nadal nieodwiedzone. Ponownie, wywołanie ```DFS-Topo``` dla 
w kończy się przed wywołanie dla *v*, a zatem *f(v)* < *f(w)* co kończy dowód części (a).

>Algorytm ```TOPOSORT``` działa w czasie liniowym. Przegląda on każdą 
krawędź dokłanie raz (od je początku), a zatem wykonuje stałą liczbę 
operacji dla każdego wierzchołka lub krawiędzie. To implikuje czas 
działania równy *O(m + n)*, a zatem (b) jest również prawdziwe.

### Po co to wszystko?

No dobrze, ale do czego w ogóle może nam się przydać uporządkowanie topologiczne grafu?
Ma ono zastosowanie przede wszystkim wszędzie tam, gdzie musimy ustalić kolejność wykonywania zadań przy zachowaniu zależności między nimi.
Częstym, szkolnym przykładem, pojawiającym się kiedy mowa jest o zastosowaniach sortowania topologicznego jest
kolejność ubierania. Podczas ubierania niektóre części garderoby muszą zostać założone przed innymi. Takie zależności można przedstawić za pomocą grafu
widocznego na rysunku 5.

![alt](/images/graf.5.png)
*Rys. 5. Graf przedstawiający zależności między częściami garderoby.*

Rysunek 6 przedstawia upożądkowanie topologiczne tego grafu. Jak widzimy przedstawia ono faktycznie sensowną, chociaż nie jedyną możliwą, kolejność zakładania ubrań.

![alt](/images/graf.6.png)
*Rys. 6. Uporządkowanie topologiczne zależności między częściami garderoby.*

Powyższy przykład, pomimo iż odwołuje się do naszych codziennych doświadczeń, jest oczywiście przykładem czysto akademickim. Sortowanie topologiczne ma jednak również
bardziej praktyczne zastosowania. Jest używane min. do harmonogramowania zadań oraz projektowania systemów w których istnieją zależności pomiędzy poszczególnymi modułami.

### *Źrodła*

[1] Roughgarden, Tim. Algorithms Illuminated: Part 2: Graph Algorithms and Data Structures. Soundlikeyourself Publishing, LLC, 2018.
[2] Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest. Wprowadzenie do algorytmów. Wydawnictwo Naukowo-Techniczne, 1998


