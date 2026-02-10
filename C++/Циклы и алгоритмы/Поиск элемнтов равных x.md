Если требуется найти, сколько в массиве `arr` элементов, равных `x`, можно использовать комбинацию `std::lower_bound` и `std::upper_bound` с этим числом. Также для такой задачи есть специальная функция — `std::equal_range(arr.begin(), arr.end(), x)`. Она возвращает два итератора — нижнюю и верхнюю границы:

```cpp
std::vector<int> arr = {1, 2, 3, 3, 4};
auto range3 = std::equal_range(arr.begin(), arr.end(), 3);
// Количество элементов, равных 3.
std::cout << std::distance(range3.first, range3.second) << std::endl; // 2 
```