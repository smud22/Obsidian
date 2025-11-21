Для поиска символа в строке используют метод строки `find`, который в качестве параметра принимает искомый символ `what` типа `char` и возвращает индекс первого вхождения этого символа в строке. Результат этого метода — число типа `std::size_t`. Если символ не найден, то возвращается специальная константа `std::string::npos`.

```cpp
std::string abcd = "Everything will be found!";

// Выведет текст: '!' index 24
std::size_t index = abcd.find('!');
// Для краткости используем хитрую тернарную операцию.
(index == std::string::npos
    ? (std::cout << "'!' not found")
    : (std::cout << "'!' index " << index)) << std::endl;

// Выведет текст: 'W' not found
index = abcd.find('W');
(index == std::string::npos
    ? (std::cout << "'W' not found")
    : (std::cout << "'W' index " << index)) << std::endl;
```

Также существует другой вариант этого метода — `find(what, from)`, — который может принимать второй параметр `from` типа `size_t` — индекс в строке, с которого нужно начать поиск символа `char what`.

```cpp
std::string abcd = "Everything will be found!";

// Найдём букву e, затем следующую e и поищем ещё e.
// К позиции предыдущей e прибавляем 1, чтобы искать 
// со следующего символа после неё.
// Иначе будем находить одну и ту же e.
std::size_t index1 = abcd.find('e');
std::size_t index2 = abcd.find('e', index1 + 1);
std::size_t index3 = abcd.find('e', index2 + 1);

// Сохраним npos для краткости.
std::size_t npos = std::string::npos;

// Получим текст 2:17:not found.
// 2 и 17 - позиции буквы e в строке.
// Используем std::to_string, чтобы в тернарной операции
// получить одинаковые типы.
std::cout << (index1==npos ? "not found" : std::to_string(index1)) << ":"
          << (index2==npos ? "not found" : std::to_string(index2)) << ":"
          << (index3==npos ? "not found" : std::to_string(index3)) << std::endl;
```

Искать по одному символу — не самая интересная операция, функция `find` может принимать не только одиночный символ, но и целую строку:

```cpp
std::string my_string = "She sells seashells by the seashore.";

std::size_t sea = my_string.find("sea");
std::size_t sea2 = my_string.find("sea", sea + 1);

// Будет выведено: 
// "The first sea is at the position 10, the second is at 27".
std::cout << "The first sea is at the position " << sea
          << ", the second is at " << sea2 << std::endl;
```

