Класс `optional` представляет собой шаблон типа, который может либо иметь значение, либо быть пустым. Чтобы лучше понять область его применения, давайте рассмотрим функцию, которая получает вектор точек `Point` и возвращает его элемент с координатой `х`, равной заданной. Вполне возможна ситуация, когда нужная точка в векторе не найдётся.

```cpp
template<typename T>
struct Point {
    T x, y;
};

template<typename T>
Point<T> FindByX(std::vector<Point<T>> points, T value_x) {
    for (auto point : points) {
        if (point.x == value_x) {
            return point;
        }
    }
    return ??? // Непонятно, что возвращать, если такой точки нет.
} 
```

Класс `optional` позволяет решить эту проблему. Мы можем вернуть из функции значение, которое будет содержать либо найденную точку, либо «ничего». Поскольку `optional` — это шаблон, при его использовании необходимо указать тип данных, который он должен содержать: `std::optional<Point<T>>`.

```cpp
#include <optional>

template<typename T>
std::optional<Point<T>> FindByX(std::vector<Point<T>> points, T value_x) {
    for (auto point : points) {
        if (point.x == value_x) {
            return point;
        }
    }
    // Если точка не найдена, возвращаем пустой optional.
    return std::nullopt; 
}

int main() {
    auto p_1 = Point<int>{1, 2};
    auto p_2 = Point<int>{3, 4};
    auto found_p = FindByX(std::vector{p_1, p_2}, 3);
    
    // Проверяем, есть ли значение.
    if (found_p) {
        // Записываем значение в переменную.
        Point<int> point = *found_p;
        std::cout << point.x << ", "s << point.y; // 3, 4.
    } else {
        std::cout << "Значение не найдено"s;
    }
} 
```

Получить значение из `optional` можно с помощью символа `*`: `*found_p`. В тех случаях, когда значение `optional` представлено классом или структурой, можно обращаться к его полям напрямую (например, `(*found_p).x`) или же использовать более лаконичную запись через стрелочку `->`.

```cpp
if (found_p) { 
    std::cout << found_p->x << ", "s << found_p->y; // 3, 4. 
```

Класс `optional` также оказывается полезен, когда нельзя выполнить функцию и вернуть из неё значение из-за некорректных параметров. Например, если в функцию `GetByIndex` передать индекс, выходящий за пределы размера вектора.

```cpp
template<typename T>
std::optional<Point<T>> GetByIndex(std::vector<Point<T>> points, int index) {
    // Если индекс выходит за границы вектора, возвращаем nullopt.
    if (index < 0) || (index >= points.size()) {
        return std::nullopt;
    }
    return points[index]; 
} 
```

Другое возможное применение `optional` — использовать его для хранения значения, которое может быть инициализировано не сразу. Так, при поиске максимального элемента в векторе его начальное значение станет известно только после того, как мы начнём перебирать элементы.

```cpp
std::optional<int> start;
for (auto x : array) {
    if (!start.has_value()) { // Если максимум ещё не присвоен.
        start = x; // Присваиваем его переменной start.
    } else {
        start = std::max(start.value(), x);
    }
} 
```