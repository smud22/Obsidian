Метод `substr` часто используют вместе с `std::find`. Кроме того, второй параметр можно опустить — в этом случае вы выведете подстроку от некоторого места и до конца.

```cpp
std::string my_string = "She sells seashells by the seashore.";

// Найдём второе слово строки. 
// Для этого найдём первые два пробела.
std::size_t first_space = my_string.find(' ');
std::size_t next_space = my_string.find(' ', first_space + 1);

// Найдём длину слова. Вычтем 1, поскольку между
// first_space и next_space есть один пробел, который нам
// не нужен.
std::size_t word_length = next_space - first_space - 1;
// Выведем слово, начиная с первого символа после пробела.
std::cout << my_string.substr(first_space + 1, word_length) 
          << std::endl;
```