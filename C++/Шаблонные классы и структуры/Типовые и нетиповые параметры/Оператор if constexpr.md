Предварительная проверка условий — отличный механизм, позволяющий сделать код быстрее и эффективнее. Чтобы убедиться, что компилятор действительно убрал избыточное ветвление из объектного кода, можно использовать специальную конструкцию `if constexpr`. Она работает так же, как обычный `if`, но гарантирует, что проверка условия будет выполнена на этапе компиляции.

```cpp
template <typename T, bool use_x>
T Sum(Point<T> point_1, Point<T> point_2) {
    if constexpr (use_x) {
        return point_1.x + point_2.x;
    } 
    return point_1.y + point_2.y;
} 
```

Внутри условия `if constexpr` можно использовать только константы, известные на этапе компиляции. Вызывать обычные функции нельзя. Но можно использовать специальные функции, помеченные словом `constexpr`. Они могут вычисляться не во время выполнения программы, а при компиляции, поэтому в условие `if constexpr` попадёт уже готовый результат.

```cpp
constexpr double ConvertToGalleons(double rubles, double rate = 100) {
    return rubles / rate;
}

template<double rubles, int galleons_limit = 10000>
bool CheckDeclaration() {
    // Функцию ConvertToGalleons можно использовать внутри constexpr if.
    if constexpr (ConvertToGalleons(rubles) > galleons_limit) {
        return true;
    }
    return false;
}

int main() {
    CheckDeclaration<50.0>(); // Вычислится на этапе компиляции.
} 
```

В следующем примере мы рассмотрим задачу, с которой нередко можно столкнуться при использовании шаблонов: функция должна работать с разными типами, но их нужно обрабатывать по-разному. Представим структуру `Triangle`, которая хранит три элемента — стороны треугольника. Они могут быть представлены числами типа `int` или же объектами класса `Side`.

```cpp
// Класс для хранения стороны фигуры.
class Side {
public:
    Side(double length): length_(length) {}
    int GetLength() const { 
        return length_; 
    }
private:
    int length_;
};

template <typename T> 
struct Triangle {
    Triangle(T a, T b, T c): a(a), b(b), c(c) {}
    T a, b, c;
}; 
```

Метод `Perimeter` суммирует стороны треугольника. Чтобы сделать это корректно, необходимо знать их тип: если он простой, можно просто обратиться к полям структуры — `triangle_1.a + triangle_2.a`. Если же это тип `Side`, для получения длины нужно использовать его метод: `triangle_1.a.GetLength() + triangle_2.a.GetLength()`. Для проверки типа используется конструкция `std::is_same_v<T, Side>`.

```cpp
template <typename T> 
struct Triangle {
    Triangle(T a, T b, T c): a(a), b(b), c(c) {}
    
    T Perimeter() const {
        // С обычным if — ошибка при подстановке int:
        // у типа int нет метода GetLength().
        if constexpr (std::is_same_v<T, Side>) {
            // Если стороны треугольника имеют тип Side,
            // нужно использовать метод GetLength().
            return a.GetLength() + b.GetLength() + c.GetLength();
        } else {
            // Иначе достаточно просто взять их значения.
            return a + b + c;
        }
    }
    
    T a, b, c;
};

int main() {
    // Стороны треугольника заданы типом int.
    std::cout << Triangle{3, 5, 4}.Perimeter() << std::endl;
    
    // Стороны треугольника заданы типом Side.
    Side side{1};
    std::cout << Triangle{side, side, side}.Perimeter()
              << std::endl;
} 
```

Конструкция `if constexpr` позволяет исключить ненужный код, оставив тот, что подходит для переданного типа. В этом случае обычный `if` привёл бы к ошибке: если компилятор оставит обе условные ветви и попытается их разобрать, то обнаружит, что у типа `int` нет метода `GetLength`.