Treść zadania
Nadchodziła Wigilia. Student Mateusz jak co roku spędzał ten czas z dziadkami we wsi pod Dołhobyczowem. Jego rodzina z tamtych stron była bardzo przywiązana do tradycji świątecznych, a przez bliskość naszych wschodnich sąsiadów tradycje polskie mieszały się tam z ukraińskimi. By zapewnić urodzaj na kolejny rok i ustrzec się przed chorobami zdarzyło się dziadkowi rozkładać świerkowe gałązki przed domem, a później porozsypywać dookoła całkiem sporo ziaren maku (Mateusz później jeszcze przez kolejne 5 miesięcy wymiatał je ze swojego żuka). Najbardziej zadziwiał go zwyczaj dmuchania na krzesło przy wigilijnym stole zanim się na nim usiadło*).

Student Mateusz do chuderlaków nie należał. Przeciwnie. Dostrzegał w tym pewien problem natury estetycznej, dlatego od pewnego czasu zaczął przywiązywać dużą wagę do liczby kalorii jakie spożywa. Przed nim wielka uczta, więc nie było to łatwe. Dlatego postanowił ułożyć wszystkie potrawy w kolejności od najmniej do najbardziej kalorycznej.

Spostrzegł również, że porcja najbardziej kalorycznej potrawy na stole mogła mieć maksymalnie tyle kalorii, ile na stole znajdowało się potraw do kwadratu minus 1. Czyli dla 12 potraw najbardziej kaloryczna porcja strawy (prawdopodobnie karpik w panierce) nie mogła mieć więcej niż 143 kcal. Na stole robiło się tłoczno od kolejnych pyszności, więc student Mateusz sięgnął po swojego laptopa i na nim kontynuował sporządzenie swojej listy. Pierwsza gwiazdka już prawie pokazała się za oknem, więc trzeba było się spieszyć!

Wejście:
Na wejściu program otrzymuje liczbę naturalna n, a w kolejnych n liniach znajduje się opis potraw. Na opis składa się jednowyrazowy napis, który jest nazwą potrawy oraz liczba naturalna, która informuje o kaloryczności tej potrawy. Nazwa i liczba kalorii oddzielone są spacją. Nazwy potraw są unikalne i żadne dwie potrawy nie mają takiej samej liczby kalorii.

2<=n<=2000000

Wyjście:
Na wyjściu program powinien wypisać listę uporządkowanych nazw potraw od najmniej do najbardziej kalorycznej.

Przykład:
Wejście:
```
6
kompot 10
sledzik 30
karpik 35
makowiec 22
pierogi 34
oplatek 5
```
Wyjście:
```
oplatek kompot makowiec sledzik pierogi karpik
```
UWAGA! Zadanie powinno działać w czasie O(n). W programie nie można używać funkcji z bibliotek STL.

kod:
```
#include <iostream>

struct food{

    int div_kcal;
    int mod_kcal;
    std::string name;

    food(int div_kcal,int mod_kcal , std::string name) : div_kcal(div_kcal), mod_kcal(mod_kcal) , name(name){};
    food(){};

};

void print_sorted(food *arr,int n){
    for(int i=0;i<n;++i){
        std::cout<<arr[i].name<<" ";
    }
}


void  counting_sort(food *arr,int n,int food::*type,int min,int max){
    int range = max - min;

    int *occurences = new int [range+1]{0};
    food *output = new food [n];

    for(int i=0;i<n;++i){
        ++occurences[arr[i].*type-min];
    }

    for(int i=1;i<range+1;++i){
        occurences[i]+=occurences[i-1];
    }

    for(int i=n-1;i>=0;--i){
        output[occurences[arr[i].*type-min]-1] = arr[i];
        --occurences[arr[i].*type-min];
    }

    delete [] occurences;

    for(int i=0;i<n;++i){
        arr[i] = output[i];
    }

    delete [] output;

}

int main() {

    std::ios_base::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout.tie(nullptr);



    int n;

    std::cin>>n;

    food *arr = new food[n];

    int kcal=0;

    bool first_occurence = true;

    int min_div,max_div=0;
    int min_mod,max_mod=0;

    for(int i=0;i<n;++i){

        std::cin>>arr[i].name>>kcal;

        arr[i].mod_kcal = kcal%n;
        arr[i].div_kcal = kcal/n;



        if(first_occurence){
            first_occurence= false;
            min_div = kcal/n;
            min_mod = kcal%n;

        }

        if(kcal/n>max_div)  max_div = kcal/n;
        if(kcal/n < min_div)  min_div = kcal/n;

        if(kcal%n>max_mod) max_mod = kcal%n;
        if(kcal%n < min_mod) min_mod = kcal%n;

    }


    counting_sort(arr,n,&food::mod_kcal,min_mod,max_mod);
    counting_sort(arr,n,&food::div_kcal,min_div,max_div);

    print_sorted(arr,n);

    delete [] arr;

    return 0;
}
```
