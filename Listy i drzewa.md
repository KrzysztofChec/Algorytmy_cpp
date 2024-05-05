Treść zadania
Mateusz wybrał się na ogólnopolski Festiwal Miłośników Fantastyki, odbywający się corocznie w Lublinie. Jest to jeden z największych i najważniejszych konwentów w Polsce. Mateusz poszedł na to wydarzenie ze swoim znajomym, któremu bardzo zależało na wygraniu pewnej nagrody. Nagrodą była limitowana edycja gry planszowej "Grzybobranie", jednak aby ją otrzymać należało rozszyfrować zbiór zaszyfrowanych wiadomości znajdujących się na jednym ze stoisk. Nasz programista, bardzo dobry i pomocny człowiek stwierdził, że rozszyfruje te wiadomości dla swojego kolegi. Tymczasem szyfry okazały się znacznie bardziej złożone, niż mógł to przewidzieć Mateusz…

Mateusz na stoisku zobaczył kilka pudełek, z których każde zawierało jedną zaszyfrowaną wiadomość. We wszystkich pudełkach znajdowały się figurki małpek, ułożone w pewnej ustalonej kolejności. Każda małpka miała napisaną na sobie literę oraz liczbę. Czujne oko naszego studenta zauważyło, że liczba na małpce mówi o ilości uchwytów, jaką posiada małpka. Do jednego uchwytu można przyczepić dokładnie jedną inną małpkę. Aby odczytać zaszyfrowaną wiadomość należy małpki ze sobą połączyć w taki sposób, żeby stanowiły swego rodzaju drzewo. Korzeniem takiego drzewa powinna być pierwsza małpka z pudełka. Kolejne małpki powinny być przymocowane w kolejności od lewej do prawej strony. Małpki przyczepiamy do pierwszego wolnego uchwytu w danym poziomie. Dopiero gdy wypełnione są wszystkie uchwyty małpek w jednym poziomie, przechodzimy do przyczepiania małpek na kolejnym poziomie. W celu otrzymania odpowiedniego ustawienia małpek, należy kolejno wyciągać je z pudełka i dopasować do odpowiednich uchwytów małpek leżących na stole. Wszystkie małpki z pudełka da się wykorzystać do tak tworzonego drzewa. Wynikiem jest ciąg liter, zapisanych na małpkach, wyświetlony w kolejności preorder.

Wejście
Na wejściu algorytm przyjmuje naturalną liczbę K, mówiącą o ilości pudełek - szyfrów do rozwiązania. Następnie program przyjmuje K liczb naturalnych N, które przedstawiają ilość małpek w ciągu. Następnie ze standardowego wejścia wczytywane jest N par: litera liczba. Jedna para reprezentuje jedną małpkę.
```
0 <= K <= 50000
0 <= N <= 50000
0 <= liczba na małpce <= 100
```
litera na małpce - tylko małe liter alfabetu łacińskiego

Wyjście
Ciąg liter dla każdego utworzonego zbioru małpek, wyświetlony w kolejności preorder.

Przykład
Wejście:
```
4
4
n 4 m 1 w 7 i 8
7
f 1 i 2 u 4 e 4 b 2 d 8 l 9
2
w 8 p 1
6
e 4 m 3 q 8 i 9 l 1 f 4
```
Wyjście:
```
nmwi
fiubdle
wp
emfqil
```

kod:

```
#include <iostream>
#include <vector>
#include <queue>

struct Node {
    char letter;
    short size;
    std::vector<Node*> children;
    Node(char letter,int size){
        this->letter=letter;
        this->size=size;
    }
};

std::string preorder(Node* node) {
    if (node == NULL)  return "";

    std::string result = std::string(1,node->letter); //zamiana chara na stringa prze zkonstruktor
    for (auto child : node->children){
        result += preorder(child);
    }

    return result;
}

int main() {

    std::ios_base::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);

    short k,n;
    std::cin>>k;

    std::string *ans = new std::string [k];
        if(k==0) return 0;

        for(int i=0;i<k;++i) {

        std::cin>>n;

        if(n==0) continue;

        std::queue<Node*> q;
        Node* root = NULL;

        for(int j=0;j<n;++j) {
            char letter;
            int size;
            std::cin>>letter>>size;

            Node *node = new Node(letter, size);

            if (root == NULL) {
                root = node;
            } else {
                while (q.empty()==false && q.front()->size == 0){
                    q.pop();
                }

                if (q.empty()==false) {
                    q.front()->children.push_back(node);
                    --q.front()->size;
                }
            }

            if (node->size>0){
                q.push(node);
                               }
        }

      ans[i] = preorder(root);
    }


    for(int i=0;i<k;++i){
        std::cout<<ans[i]<<std::endl;
    }

        delete [] ans;
    return 0;
}
```
