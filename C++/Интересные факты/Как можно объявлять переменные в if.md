Аргумент, переданный по ссылке, можно использовать, чтобы вернуть из функции дополнительное значение, помимо основного. Аргумент, который функция принимает по ссылке и использует только для записи, называется **выходным аргументом** (output argument).

В этом примере функция `Invert` принимает ссылку на булеву переменную, сигнализирующую об ошибке:

```cpp
#include <iostream>
#include <cmath>

// Возвращает число, обратное number, то есть 1 / number.
// Произведение числа на обратное ему даёт единицу.
// В переменную, переданную в параметре was_error, будет записан признак ошибки.
double Invert(double number, bool& was_error) {
    double result = 1. / number;
    
    // Функция std::isfinite проверяет, что аргумент – это
    // корректное число double, отличное от NaN и бесконечности. 
    was_error = !std::isfinite(result);
    return was_error ? 0.0 : result;
}

using namespace std::literals;

int main() {
    double number;
    std::cin >> number;
    bool was_error;
    if (double inv_number = Invert(number, was_error); !was_error) {
        std::cout << inv_number << std::endl;
    } else {
        std::cout << "Error"s << std::endl;
    }
} 
```

Если запустить эту программу и ввести число 2, программа выведет обратное ему число — 0.5, так как 0.5 * 2 = 1. Если ввести 0, программа выведет строку `Error`, так как не существует числа, которое при умножении на ноль давало бы единицу.