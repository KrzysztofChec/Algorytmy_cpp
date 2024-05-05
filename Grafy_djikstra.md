Zadanie:
Flota Federacji składa się z pewnej liczby statków i jest wyposażona w system teleportacyjny. Jeśli pomiędzy dwoma statkami we flocie istnieje szlak komunikacyjny, to możliwe jest przemieszczenie się z jednego do drugiego (wykonanie skoku) przy pomocy transportera. Czasami, aby przeteleportować się do wybranej jednostki Federacji trzeba wykonać kilka skoków pośrednich. Każda teleportacja pomiędzy statkami x i y zajmuje powien czas t. Jednak jeśli ktoś inny akurat materializuje się w docelowym teleporcie, to czas dotarcia wydłuża się o 1 sekundę. Dotarcie dwóch osób w tym samym czasie skutkowałoby nieodwracalnym scaleniem się, poza tym poprzednia osoba, która właśnie dociera do statku musi mieć czas na zejście z platformy transportera.

Kapitan Kirk znajduje się na statku Enterprise oznaczonego numerem 0, a Spock na statku o numerze s. Kapitan Kirk potrzebuje jak najszybciej swojego pierwszego oficera u swojego boku. Jaki jest najkrótszy czas, po którym Spock powróci na Enterprise?

Wejście:
W pierwszej linii wejścia znajduje się liczba naturalna d, oznaczająca liczbę testów. Po niej następuje d zestawów danych opisujących połączenia teleportacyjne pomiędzy statkami we flocie. W pierwszej linii każdego z zestawów są liczby naturalne n, m i s, gdzie n jest liczbą statków, m liczbą możliwych teleportacji pomiędzy nimi, a s numerem statku, na którym znajduje się Spock.

W kolejnych m liniach znajdują się 3 liczby naturalne x, y i t, gdzie x i y są numerami statków, pomiędzy którymi może odbyć się teleportacja, a t jest czasem, jaki jest potrzebny na teleportację. Czas wyrażony jest w sekundach. Zakładamy, że pary numerów statków są unikalne oraz że zawsze istnieje ścieżka pomiędzy statkiem, na którym jest Spock a Enterprise. Teleportacja może odbywać się w obu kierunkach.

W następnych n liniach znajduje się liczba naturalna k, a po niej k liczb naturalnych. Kolejne linie odpowiadają numerom statków, a podane w liniach liczby są uporządkowane. Każda z k liczb wyraża czas w sekundach od chwili startu Spocka i informuje, że jeśli Spock spróbuje pojawić się na danym statku w tej sekundzie, to musi poczekać na materializację, bo teleport zajmowany jest przez kogoś innego. Spock może się tam pojawić dopiero po upływie sekundy (lub później, jeśli teleport ponownie ktoś zajmuje).

Przykład: Dotychczasowa droga zajęła Spockowi 2 sekundy. Chce on się teleportować do kolejnego statku, co zajmie mu 3 sekundy. Docelowy transporter będzie zajęty w 5 i 6 sekundzie. Wydłuża to czas teleportacji i Spock będzie na miejscu po 7 sekundach od startu.
```
1<=d<=100
2<=n<=1000
0<=k<=10000
```
Wyjście:
Na wyjściu dla każdego zestawu program ma wypisać najkrótszy czas, po którym Spock dotrze do Enterprise.

Przykład:
Wejście:
```
2
4 6 2
0 1 3
0 2 10
0 3 2
1 2 3
1 3 1
2 3 2
1 10
0
0
3 2 3 4
3 3 1
0 1 1
0 2 1
1 2 1
1 1
0
0
```
Wyjście:
```
6 
2
```

kod:
```
#include <iostream>
#include <vector>
#include <queue>
struct Node{
    long index;
    long distance;
    Node(long a,long time){
        this->index=a;
        this->distance=time;
    }
    bool operator<(const Node &next) const{
        return (this->distance > next.distance);
    }
};

class Graph{

    long size;
public:

    std::vector<std::vector<Node>> connection_list;

    Graph(int amount_of_nodes) : size(amount_of_nodes), connection_list(amount_of_nodes) {}

    void add_connection(Node node1,Node node2){
        connection_list[node1.index].push_back(node2);
        connection_list[node2.index].push_back(node1);
    }
    long get_size(){
        return size;
    }
};


long long djikstra(  Graph graph,int starting_index,std::vector<std::vector<int>> delay,std::vector<bool> index_to_check){

    std::vector<long long> distance(graph.get_size(),9223372036854775807);
    distance[starting_index]=0;

    std::priority_queue<Node> pq;

    pq.emplace(starting_index,0);

    while (!pq.empty()){
        Node temp_node = pq.top();
        pq.pop();

        for(Node &elemet : graph.connection_list[temp_node.index]){
            int distance_from_start = temp_node.distance+elemet.distance;

            if (index_to_check[elemet.index]) {
                for(int i=0;i<delay[elemet.index].size();++i){
                    if(distance_from_start==delay[elemet.index][i]){
                        ++distance_from_start;
                    }
                }
            }

            if( distance_from_start < distance[elemet.index]){
                distance[elemet.index]=distance_from_start;
                pq.emplace(elemet.index,distance_from_start);
            }
        }

    }

    return distance[0];
}

int main() {

    std::ios_base::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);

    int amount_of_tests;
    int connections_amount,target_ship,node_amount;

    long index_a,index_b,time_between;
    bool check=false;
    std::cin>>amount_of_tests;

    if(amount_of_tests > 100 || amount_of_tests < 1) return 0;

    for(int i=0;i<amount_of_tests;++i){
        std::cin>>node_amount>>connections_amount>>target_ship;

        if(node_amount < 2 || node_amount > 1000) continue;

        std::vector<std::vector<int>> time_to_add(node_amount);
        Graph graph (node_amount);
        std::vector<bool> index_to_check(node_amount, false);


        for(int j=0;j<connections_amount;++j){
            std::cin>>index_a>>index_b>>time_between;
            graph.add_connection(Node(index_a,time_between),Node(index_b, time_between));
        }


        for(int j=0;j<node_amount;++j){
            long o;
            std::cin>>o;
            if(o<0 || o >10000) continue;
            for(int k=0;k<o;++k){

                if(!check){
                    index_to_check[j]= true;
                    check= true;
                }

                long temp;
                std::cin>>temp;
                time_to_add[j].push_back(temp);
            }
            check= false;
        }


        std::cout<<djikstra(graph,target_ship,time_to_add,index_to_check)<<std::endl;

        time_to_add.clear();
        index_to_check.clear();
    }

    return 0;
}
```
