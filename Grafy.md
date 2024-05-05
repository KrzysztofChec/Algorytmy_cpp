Do siedziby operacyjnej Partycji dotarła wiadomość od jednego z jej tajnych informatorów. Miał do przekazana bardzo obiecujące informacje, który mogły w znacznym stopniu przyczynić się do ujawnienia rządowego spisku. Podał miejsce spotkania, jednak zastrzegł, by po mieście poruszać się z dużą ostrożnością i unikać monitorowanych miejsc.

Partycja zaczęła studiować mapę miasta, o wymiarach nxm. Podzieliła ją na n rzędów i m kolumn, a każdy tak powstały fragment mapy oznaczyła w następujący sposób:
```
. - miejsce z możliwością przejazdu
# - miejsce zablokowane bez możliwości przejazdu
M - miejsce z możliwością przejazdu, monitorowane
S - siedziba operacyjna
X - miejsce spotkania
Na początku postanowiła wyznaczyć najkrótszą drogę z siedziby do miejsca spotkania i sprawdzić ile miejsc monitorowanych się na tej drodze znajduje. Przez najkrótszą drogę rozumiana jest liczba miejsc odwiedzonych pomiędzy siedzibą a miejscem spotkania. Po mapie z danego miejsca można się poruszać na sąsiadujące pola w górę, w dół, lewo lub prawo (o ile pola te nie są zablokowane). W przypadku znalezienia więcej niż jednej najkrótszej drogi o tej samej długości, pierwszeństwo ma ta z mniejszą liczbą miejsc monitorowanych.
```
Wejście
W pierwszej linii wejścia znajdują się dwie liczby naturalne n i m, oznaczające wymiary mapy. W kolejnych liniach znajduje się reprezentacja mapy zgodna z opisem. Należy przyjąć, że zawsze istnieje droga prowadząca od siedziby do miejsca spotkania.

1 ≤ n,m ≤ 1000
Wyjście
Na wyjściu program ma wypisać dwie liczby naturalne oznaczające długość najkrótszej drogi do punktu docelowego oraz liczbę napotkanych miejsc monitorowanych.

Przykład
Wejście:
```
5 8
#..M.M#X
S..##...
.#.M...#
...#.#.#
####..M#
```
Wyjście:
```
10 1
```

kod:
```
#include <iostream>
#include <vector>
#include <queue>


short move_x[4] = {-1, 0, 1, 0};
short move_y[4] = {0, 1, 0, -1};


const int INF = 65535;

struct Node {
    int position_x,positin_y;
    int distance_from_start,m_counter;

    Node(int px,int py,int dist,int m_count ){
        position_x=px;
        positin_y=py;
        distance_from_start=dist;
        m_counter=m_count;
    }

    bool operator<(const Node& rhs) const {
        if(distance_from_start != rhs.distance_from_start) return distance_from_start > rhs.distance_from_start;
        return m_counter > rhs.m_counter;
    }
};

int main()
{

    std::ios_base::sync_with_stdio(false);
     std::cin.tie(nullptr);
     std::cout.tie(nullptr);

    short n,m;
    std::cin>>n>>m;

    std::vector<std::vector<char>> map(n,std::vector<char>(m));
    unsigned short start_x,start_y,end_x,end_y;

    char c;
    for(unsigned short i=0;i<n;++i){
        for(unsigned short j=0;j<m;++j){

            std::cin>>c;
            if(c=='S'){
                start_x=i;
                start_y=j;
            }
            if(c=='X'){
                end_x=i;
                end_y=j;
            }
           map[i][j] = c;
        }
    }

       std::vector<std::vector<int>> distance(n,std::vector<int>(m,INF));
       std::vector<std::vector<int>> M(n,std::vector<int>(m,0));
       std::priority_queue<Node> pq;

      distance[start_x][start_y] =0;
      pq.push(Node(start_x,start_y,0,0));

      while(pq.empty()==false){
          Node temp = pq.top();

          pq.pop();

          int current_x,current_y,distance_from_a_starting_point,M_counter;

                    distance_from_a_starting_point=temp.distance_from_start;
                    M_counter=temp.m_counter;
                    current_x=temp.position_x;
                    current_y=temp.positin_y;


          for(short i=0;i<4;++i){
              int new_x=current_x+move_x[i];
              int new_y= current_y+move_y[i];

              if(new_x >=0 && new_x < n && new_y >= 0 && new_y<m &&  map[new_x][new_y]!='#'){
                  int new_M=M_counter;
                  if(map[new_x][new_y]=='M'){
                    new_M = M_counter + 1 ;
                  }

                  if(distance_from_a_starting_point+1 < distance[new_x][new_y] || (distance_from_a_starting_point+1 == distance[new_x][new_y] && new_M < M[new_x][new_y])){
                      distance[new_x][new_y] = distance_from_a_starting_point+1; //dodajemy +1 bo ten wierzcholek jest oddalony o 1 kratke w jakas strone
                      M[new_x][new_y] = new_M;
                      pq.push(Node(new_x,new_y,distance[new_x][new_y],M[new_x][new_y]));
                  }

              }

          }

      }

      std::cout<<distance[end_x][end_y]<<" "<<M[end_x][end_y]<<std::endl;

    return 0;
}
```
