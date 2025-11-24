А ещё разбиение программы на функции облегчает её тестирование. Написав очередную функцию, вы можете проверить её работу, используя макрос [`assert`](https://en.cppreference.com/w/cpp/error/assert). Для его использования подключите файл `<cassert>`. Макрос `assert` проверяет значение своего аргумента и, если оно ложно или равно нулю, выводит диагностическое сообщение и завершает работу программы. В противном случае он ничего не делает.

```cpp
#include <cassert>
#include <iostream>

int main() {
    assert(2 * 2 == 4); // OK.
    assert(1);          // OK.
    assert(2 * 2 == 5); // Выведет сообщение и завершит программу.
    // Этот код выполняться не будет.
    std::cout << "Hello"s << std::endl;
} 
```

Чтобы проверить функцию `CountItemsGreaterOrEqual`, нужно вызвать её несколько раз с разными параметрами и убедиться, что возвращённый результат совпадает с ожидаемым.

```cpp
#include <cassert>
#include <vector>

int CountItemsGreaterOrEqual(std::vector<int> numbers, int value) {
    ...
}

int main() {
    assert(CountItemsGreaterOrEqual({}, 42) == 0);

    const std::vector<int> numbers{5, 2, 8, 3, 4, 5}; 

    assert(CountItemsGreaterOrEqual(numbers, 2) == 6);
    assert(CountItemsGreaterOrEqual(numbers, 4) == 4);
    assert(CountItemsGreaterOrEqual(numbers, 8) == 1);
    assert(CountItemsGreaterOrEqual(numbers, 9) == 0);
} 
```