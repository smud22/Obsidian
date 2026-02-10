Вы уже использовали словари для организации счётчиков для подсчёта количества вхождений. Например, для определения анаграмм нужно было подсчитать количество букв в двух словах и затем сравнить два словаря с этими данными между собой. Функция, реализующая подсчёт букв в слове, могла выглядеть так:

```cpp
std::map<char, int> CountChars(const std::string& word) {
    std::map<char, int> counter;
    for (char c : word) {
        auto it = counter.find(c);
        if (it == counter.end()) {
            counter.insert({c, 1});
        } else {
            it->second++;
        }
    }
    return counter;
} 
```

Это верная реализация. Но у неё есть недостаток: если ключа нет в `counter`, она делает поиск два раза. Первый раз в методе `find` и второй раз в методе `insert`, когда `std::map` ищет место, куда вставить новое значение. У метода `insert` контейнеров `std::set` и `std::map` есть перегрузка `insert(iterator, value)`, которая принимает на вход итератор. Этот итератор должен указывать на позицию сразу после места, куда должен вставиться `key`. Можно использовать `lower_bound` и делать поиск только один раз. Он вернёт или итератор на искомый элемент, или итератор на следующий элемент. Следующий элемент — это как раз то, что ожидается как итератор в `insert`. В таком случае `insert` будет знать, куда добавлять новый элемент, и не будет делать поиск.

![](https://code.s3.yandex.net/CPP/assets/c01_2024/s04_containers_and_algorithms/image_cc-axpl.jpg)

Такая реализация будет делать поиск в `std::map` только один раз — в методе `lower_bound`:

```cpp
std::map<char, int> CountChars(const std::string& word) {
    std::map<char, int> counter;
    for (char c : word) {
        auto it = counter.lower_bound(c);
        // Если c нет в counter, 
        // то it будет указывать на counter.end() или на следующий элемент.
        if (it == counter.end() || it->first != c) {
            // it используется как подсказка для вставки нового элемента.
            counter.insert(it, {c, 1});
        } else {
            it->second++;
        }
    }
    return counter;
} 
```