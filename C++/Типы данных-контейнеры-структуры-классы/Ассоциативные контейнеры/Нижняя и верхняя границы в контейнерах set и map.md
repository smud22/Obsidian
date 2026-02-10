Функциям `std::lower_bound` и `std::upper_bound` для эффективной работы требуются итераторы с произвольным доступом (как в векторе). Итераторы `std::map` и `std::set` такими не являются, но данные в этих контейнерах отсортированы, поэтому у них есть собственные методы поиска `lower_bound` и `upper_bound`.

Поиск нижней и верхней границы у `std::set`:

```cpp
std::set<int> s = {0, 1, 3, 5, 7};
std::cout << *(s.lower_bound(1)) << ::std::endl; // 1
std::cout << *(s.upper_bound(1)) << ::std::endl; // 3
std::cout << *(s.lower_bound(4)) << ::std::endl; // 5
std::cout << *(s.upper_bound(4)) << ::std::endl; // 5 
```

У `std::map` методы те же, но ищут они нижнюю и верхнюю границы среди ключей:

```cpp
std::map<int, int> m = {{0, 0}, {1, 0}, {3, 8}, {5, 0}, {7, 9}};
const auto it = m.upper_bound(1);
std::cout << it->first << ": "s << it->second << std::endl; // 3: 8
std::cout << m.lower_bound(7)->second << std::endl; // 9
std::cout << (m.upper_bound(7) == m.end()) << std::endl; // 1 
```

В методах `lower_bound` и `upper_bound` контейнеров `std::map` и `std::set` задавать свой компаратор нельзя. Используется компаратор, заданный при создании контейнера.