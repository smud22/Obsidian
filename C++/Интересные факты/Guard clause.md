В примере была проверка в начале функции. Подобные проверки в англоязычных источниках называют guard clause (англ. «охранное условие»). В них обрабатывают специальные случаи и завершают работу функции. Уже после идёт основная логика. Использование такой валидации позволяет сделать код более понятным и сократить вложенность блоков.

```cpp
void PrintQuotient(int dividend, int divisor) {
    if(!divisor) {
        std::cerr << "Can not divide by zero" << std::endl;
        return;
    }
    std::cout << dividend << " / " << divisor << " = " << dividend / divisor    << std::endl;
}
```
