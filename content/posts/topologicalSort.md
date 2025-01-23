+++
date = '2025-01-23T12:28:17+01:00'
draft = true
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

W pierwszym kroku algorytm oznacza wszystkie węzły w grafie jako nieodwiedzone. Następnie definiowana jest zmienna ```curLabel``` której wartość ma być równa ilości węzłów w sortowanym grafie. Zmienna ta ma kluczowe znaczenie dla działania algorytmu, ponieważ jest wykorzystywana do przypisywania wierzchołką wartości *f*. W kolejnym kroku, dla każdego wierzchołka grafu wywoływana jest funkcja ```DFS-Topo```. Oznacza ona wierzchołek jako odwiedzony oraz wywołuje się rekurencyjnie dla wszystkich nieodwiedzonych wierzchołków na liście sąsiedztwa pierwotnego wierzchołka. Ostatecznie, kiedy zakończą się wszystkie wywołania dla wierzchołków z listy sąsiedztwa, funkcja przypisuje wierzchołkowi wartość *f* równą aktualniej wartości ```curLabel```, a wartość zmiennej ```curLabel``` jest zmniejszana o 1. To czemu wartość *f* jest przypisywana wierzchołką w taki właśnie sposób wyjaśnie w dalszej części artykułu, będącej analizą poprawności i złożoności algorytmu. Natomiast w tej części chciałbym zaprezentować jeszcze implementację opisanego wcześniej lagorytmu w języku Python.