Wejście:
W pierwszej linii wejścia podane są 4 liczby naturalne n, m, s i d, gdzie n jest liczbą zamków, m liczbą dróg pomiędzy zamkami, s numerem zaatakowanego zamku, a d liczbą dni potrzebnych wojskom Lammisterów na dotarcie do Riverunn.

W kolejnych m liniach podane są pary liczb naturalnych odpowiadających numerom zamków, pomiędzy którymi znajduje się trakt.

Wyjście:
Na wyjściu program ma wypisać za ile dni od wysłania kruków ostatni z nich doleci do celu oraz ile zamków będzie w stanie wysłać pomoc do Riverunn na czas.

2<=n<=1000

Przykład:
Wejście:
```
7 6 0 5
1 0
0 2
2 3
3 4
2 5
2 6
```
Wyjście:
```
3 5
```

kod:

```
#include <iostream>
#include <list>
#include <vector>
#include <queue>

class Castles {

    long amount_of_v;
 std::vector<std::vector<unsigned int>> list_ptr;
    long max_size ;

public:

    int amount_of_castles_to_help=0;
    int days=0;
    int crows=0;
    Castles(long amount_of_v,long days,long max_size){
        this->amount_of_v = amount_of_v;
        list_ptr = std::vector<std::vector<unsigned int>>(amount_of_v);
        this->days = days;
        this->max_size = max_size;
    }

    void add_v(long v, long w){
        list_ptr[v].push_back(w);
        list_ptr[w].push_back(v);//robimy na zasdiedzie polaczenie 2 nidow zeby sie potem nie cofac
    }

    void bfs(long v){
        std::vector<bool> visited(amount_of_v,false);
        std::vector<int> level(amount_of_v, -1);

            std::queue<int> q;
        q.push(v);
        level[v] = 0;

            long temp=0;

        while (q.empty()==false){
            temp = q.front();
            q.pop();

            visited[temp] =true;

            for (unsigned int &element_of_a_list : list_ptr[temp]){  //przesakujemy po kazdym elemencie z lity

                    if (visited[element_of_a_list]==true) continue;
                    level[element_of_a_list]= level[temp]+1;
                    crows =level[temp]+1;
                    if(level[element_of_a_list] <= days/2){
                        ++amount_of_castles_to_help;
                    }
                     visited[element_of_a_list] = true;
                q.push(element_of_a_list);
            }

        }

        if(days==0){
            std::cout<<crows<<" "<<0<<std::endl;
        }else
        std::cout<<crows<<" "<<amount_of_castles_to_help<<std::endl;

    }


};

int main() {


    std::ios_base::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);

    int amount_of_castles;
    long size,attacked_castle,days_to_pass;


    std::cin>>amount_of_castles>>size>>attacked_castle>>days_to_pass;
     if(attacked_castle<0) return 0;

    Castles castle(amount_of_castles,days_to_pass,size);

    long  c1,c2;
    for(unsigned int i=0;i<size;++i){

        std::cin>>c1>>c2;
        if(c1 < 0 || c2 < 0) return 0;
        if(c1==c2) return 0;
        castle.add_v(c1,c2);

    }
        castle.bfs(attacked_castle);

    return 0;
}
```
