В С++ операции могут применяться не только к примитивным типам, но и к объектам классов, в том числе пользовательских. Рассмотрим на примере шаблона `std::pair`.

Шаблон `pair` позволяет хранить два значения в одном объекте. Он похож на структуру с двумя полями, но позволяет не писать её объявление. Посмотрите на пример со структурой:

```cpp
#include <iostream>

// Создадим структуру с двумя полями: first и second.
struct MyPair {
    std::string first;
    int second;
};

int main() {
    MyPair person1{"Ivan"s, 22};
    
    std::cout << person1.first << std::endl;
    std::cout << person1.second << std::endl;
} 
```

А вот так можно переписать, используя `pair`:

```cpp
// Подключим utility, чтобы использовать pair.
#include <utility>
#include <iostream>

int main() {
    std::pair<std::string, int> person2{"Alexey"s, 20};
    
    std::cout << person2.first << std::endl;
    std::cout << person2.second << std::endl;
} 
```

Короче, не правда ли?

Основное отличие от структуры в том, что поля `pair` всегда называются `first` (англ. «первый») и `second` (англ. «второй»), а у структуры полям можно дать любые имена.

Контейнер `pair` — это простой контейнер, определённый в заголовке `<utility>` и состоящий из двух элементов: примитивных значений или объектов.

Однако использовать `pair` можно не только для хранения пары значений. Один из интересных сценариев использования — возможность возвращать из функции сразу два значения. Посмотрите на пример:

```cpp
#include <iostream>
#include <utility>

using namespace std::literals;

// Функция находит первую гласную букву.
// Возвращает букву и её позицию в строке.
std::pair<char, size_t> FindVowel(std::string str) {
    size_t pos = str.find_first_of("aeoiuy"s);
    if (pos == str.npos) {
        // Ничего не нашли: возвращаем пару с 
        // нулевым символом.
        return {'\0', pos};
    }
    
    // Нашли гласную: возвращаем саму гласную и её позицию
    // в виде пары.
    return {str[pos], pos};
}

int main() {
    auto [first, second] = FindVowel("Here the first vowel is 'e'!"s);

    // Выведем элементы пары.
    std::cout << first << " "s
         << second << std::endl;
} 
```

Код, представленный выше, можно упростить при помощи автоматического выведения типов. При этом даже не нужно указывать параметры этого темплейта.

```cpp
#include <algorithm>
#include <iostream>
#include <utility>

using namespace std::literals;

// Определим пару.
auto GetCount(char lookup_ch, std::string str) {
    return std::pair{lookup_ch, std::count(str.begin(), str.end(), lookup_ch)};
}

int main() {
    auto [first, second] = GetCount('c', "Here exactly 3 chars 'c'!"s);

    // Выведем элементы пары.
    std::cout << first << " "s
         << second << std::endl;
} 
```

При помощи вложенности можно передать из функции и три параметра. Но развернуть их в отдельные переменные будет уже не так просто.

Для передачи более двух аргументов из функции лучше использовать `std::tuple<…>`. О нём вы узнаете в следующей теме.

```cpp
#include <iostream>
#include <utility>

using namespace std::literals;

std::pair<int, std::pair<char, int>> ReadBinOp(std::istream& stream) {
    int left_op;
    char op;
    int right_op;
    stream >> left_op;
    stream >> op;
    stream >> right_op;
    
    // Возвращаем пару из числа и пары.
    return std::pair{left_op, std::pair{op, right_op}};
}

int main() {
    auto [first, secondPair] = ReadBinOp(std::cin);
    auto [second, third] = secondPair;

    // Выведем элементы пары.
    std::cout << "first: "s << first << " "s;
    std::cout << "op: "s << second << " "s;
    std::cout << "second: "s << third << std::endl;
} 
```

```cpp
using IntPair = std::pair<int, int>;

// Складывает покомпонентно две пары.
IntPair Add(const IntPair& vec1, const IntPair& vec2) {
    return IntPair{vec1.first + vec2.first, vec1.second + vec2.second};
}

// Вычитает покомпонентно две пары.
IntPair Sub(const IntPair& vec1, const IntPair& vec2) {
    return IntPair{vec1.first - vec2.first, vec1.second - vec2.second};
} 
```

### Лексикографическое сравнение и пары

Для пар определены операции сравнения. Например, `pair("ann"s, 15) < pair("bob"s, 42)`. Сравнение ориентируется на первые элементы пары. Так, например, `pair("ann"s, 100000) < pair("bob"s, 1)`, несмотря на то что 100000 > 1, а `pair("bob"s, 1) > pair("ann"s, 100000)`. Вторые элементы принимаются во внимание, только если первые элементы равны: `pair("ann"s, 100000) > pair("ann"s, 1)`, `pair("ann"s, 15) < pair("ann"s, 10000)`.