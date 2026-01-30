
|   |   |   |
|---|---|---|
|Defined in header `[<tuple>](https://en.cppreference.com/w/cpp/header/tuple.html "cpp/header/tuple")`|||
|template< class... Types >  <br>class tuple;||(since C++11)|
||||

Class template `std::tuple` is a fixed-size collection of heterogeneous values. It is a generalization of [std::pair](https://en.cppreference.com/w/cpp/utility/pair.html "cpp/utility/pair").

If [std::is_trivially_destructible](https://en.cppreference.com/w/cpp/types/is_destructible.html)<Ti>::value is true for every `Ti` in `Types`, the destructor of `std::tuple` is trivial.

If a program declares an [explicit](https://en.cppreference.com/w/cpp/language/template_specialization.html "cpp/language/template specialization") or [partial](https://en.cppreference.com/w/cpp/language/partial_specialization.html "cpp/language/partial specialization") specialization of `std::tuple`, the program is ill-formed, no diagnostic required.