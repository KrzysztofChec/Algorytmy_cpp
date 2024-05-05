### Zadanie:
Janusz od zawsze był wielkim koneserem polskiej kuchni. Drugie danie w jego domu składało się obowiązkowo z ziemniaków, kotleta i jakiegoś rodzaju szeroko pojętej surówki. Pan domu jednak z przykrością zauważył, że dieta taka kiepsko wpływa na jego wytrenowany mięsień piwny przekształcając go efektywnie w słoniową ciążę spożywczą. Postanowił więc zrobić listę n najczęściej jedzonych produktów wraz z ich kalorycznością i zastanowić się które z nich powinien ze sobą zestawić, by w porze obiadu zjeść dokładnie k kalorii (zakładamy, że dany produkt może być wykorzystany podczas obiadu co najwyżej raz).

Janusz swoje przemyślenia przekazał żonie, ale to jak Grażyna zareagowała na instruowanie jej jak ma gotować przemilczymy.

Wejście
Na wejściu program otrzymuje liczbę naturalną t, oznaczającą liczbę testów. Następnie podanych jest t zestawów danych. Na jeden zestaw danych składa się liczba naturalna n, która oznacza liczbę produktów. Następnie podanych jest n liczb naturalnych dodatnich, które informują o liczbie kalorii kolejnych produktów. W ostatniej linii testu podana jest liczba naturalna dodatnia k - liczba kalorii, którą pragnie spożyć Janusz.
```
1 ≤ t ≤ 10
1 ≤ n ≤ 1000
```
Wyjście
Na wyjściu należy dla każdego testu wypisać liczbę naturalną informującą na ile sposobów można użyć produktów z listy.

Przykład
Wejście:
```
2
6 80 40 40 120 160 80 
160 
3 100 110 120 
130
```
Wyjście:
```
6
0
```
Uwaga! Zadanie ma być rozwiązane metodą przeszukiwania wyczerpującego z nawrotami.
kod :
```
#include <iostream>
#include <vector>
#include <algorithm>

bool comparator(short &right,short &left){
    return right<left;
}

void sort(std::vector<short> &arr,short &n){
    std::sort(arr.begin(),arr.begin()+n, comparator);
}

void enter_food(std::vector<short> &arr,short &n){
    for(short j=0;j<n;++j){
        std::cin>>arr[j];
    }
}

std::vector<std::vector<int>> visited;

int backtracking(short pos,int max_kcal,std::vector<short> &arr){
    if(max_kcal==0){
        return 1;
    } else if(max_kcal<0 || pos>= arr.size()){
        return 0;
    }

    if(visited[pos][max_kcal] != -1) {
        return  visited[pos][max_kcal];
    }

    int amount_of_dishes = 0;

    for(short i=pos;i<arr.size();++i){
        if(arr[i]<= max_kcal){
           amount_of_dishes += backtracking(i+1,max_kcal-arr[i],arr);
        } else{
            break;
        }
    }

    visited[pos][max_kcal] = amount_of_dishes;
    return amount_of_dishes;

}



int main() {

    short t,n;
    std::cin>>t;
    int max_kcal;
    int result=0;
    std::vector<short> arr;

    for(short i=0;i<t;++i){
        result=0;

        std::cin>>n;
        arr.clear();
        arr.resize(n);

        enter_food(arr,n);
        sort(arr,n);
        std::cin>>max_kcal;


        visited.clear();
        visited.resize(n, std::vector<int>(max_kcal + 1, -1));

       result= backtracking(0,max_kcal,arr);

        std::cout<<result<<std::endl;

        result=0;

    }

    return 0;
}
```
