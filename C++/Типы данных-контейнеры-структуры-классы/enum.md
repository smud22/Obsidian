# Пример
```cpp
enum class Month { Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec };

int main() {
	enum class Finger {
		Thumb,
		Index,
		Middle,
		Ring,
		Pinky
	}; 
	// Воспользуемся квадроточием, чтобы записать 
	// в переменную значение типа Finger.
	Finger some_finger = Finger::Pinky; // some_finger = 4
	}
```

Перечисление создаётся при помощи ключевых слов `enum class`, далее идёт имя типа, и в фигурных скобках через запятую перечисляются значения. В конце обязательно ставится точка с запятой. При этом будет создан тип, обладающий фиксированным набором значений. Чем-то он похож на тип `bool`, обладающий двумя значениями, но `bool` встроен в язык, а тип-перечисление создаёте вы сами.

```cpp
int n = 4;
Month m = static_cast<Month>(n);

bool isMay = (m == Month::May);
// В isMay значение true.
// В enum получилось, что Май – четвёртый месяц,
// ведь отсчёт начинается с нуля.

int k = static_cast<int>(Month::Jan); 
// В k значение 0.
```

Обратите внимание на скобки при использовании [`static_cast`](static_cast): целевой тип указывается в треугольных скобках, а исходная переменная или значение — после в круглых.

Текстовые названия значений остаются только в коде программы. Использовать их для ввода или вывода значения `enum`-типа или для преобразования его в строку не получится.

Нумерацию в перечислениях можно указать явно:

```cpp
enum class Day {
    Mon = 1,
    Tue = 2,
    Wed = 3,
}
```

Или задать явно начальное значение, а компилятор пронумерует подряд остальное:

```cpp
enum class Day {
    Mon = 1,
    Tue,    // static_cast<int>(Day::Tue) == 2.
    Wed,    // static_cast<int>(Day::Wed) == 3.
}
```

Часто `enum`-типы используются в `switch` так:

```cpp
enum class Weekday { Mon = 1, Tue, Wed, Thu, Fri, Sat, Sun };

int day_number;
std::cout << "Введите номер дня:" << std::endl;
std::cin >> day_number;
Weekday week_day = static_cast<Weekday>(day_number);

switch (week_day) {
    case Weekday::Mon:
    case Weekday::Tue:
    case Weekday::Wed:
    case Weekday::Thu:
    case Weekday::Fri:
        std::cout << "Будний день" << std::endl;
        break;
    case Weekday::Sat:
    case Weekday::Sun:
        std::cout << "Выходной день" << std::endl;
        break;
    default:
        std::cout << "Введён некорректный день" << std::endl;
        break;
}
```