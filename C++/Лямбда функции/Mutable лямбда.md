```cpp
std::vector<int16_t> signal;
signal.reserve(48000);

const double amplitude = 15000;
const int wave_length = 200;

// Используем mutable-лямбда функцию. Она может менять захваченные значения.
// В данном случае меняем i, который инициализируем в 
// квадратных скобках: i = 0.
auto generator = [=, i = 0]() mutable {
    // Возвратим значение. Алгоритм generate_n добавит его
    // в вектор.
    return amplitude * sin(2 * i++ * std::numbers::pi / wave_length);
};

std::generate_n(std::back_inserter(signal), wave_length, generator);
```
