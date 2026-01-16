|   |   |   |
|---|---|---|
|Defined in header `[<numeric>](https://en.cppreference.com/w/cpp/header/numeric.html "cpp/header/numeric")`|||
|template< class M, class N >  <br>constexpr [std::common_type_t](https://en.cppreference.com/w/cpp/types/common_type.html)<M, N> gcd( M m, N n );||(since C++17)|
||||

Computes the [greatest common divisor](https://en.wikipedia.org/wiki/greatest_common_divisor "enwiki:greatest common divisor") of the integers m and n.

If either `M` or `N` is not an integer type, or if either is (possibly cv-qualified) bool, the program is ill-formed.

If either |m| or |n| is not representable as a value of type [std::common_type_t](https://en.cppreference.com/w/cpp/types/common_type.html)<M, N>, the behavior is undefined.

### Parameters

|   |   |   |
|---|---|---|
|m, n|-|integer values|

### Return value

If both m and n are zero, returns zero. Otherwise, returns the greatest common divisor of |m| and |n|.

### Exceptions

Throws no exceptions.

### Notes

|[Feature-test](https://en.cppreference.com/w/cpp/utility/feature_test.html "cpp/utility/feature test") macro|Value|Std|Feature|
|---|---|---|---|
|[`__cpp_lib_gcd_lcm`](https://en.cppreference.com/w/cpp/experimental/feature_test.html#cpp_lib_gcd_lcm "cpp/feature test")|[`201606L`](https://en.cppreference.com/w/cpp/compiler_support/17.html#cpp_lib_gcd_lcm_201606L "cpp/compiler support/17")|(C++17)|`std::gcd`, [std::lcm](https://en.cppreference.com/w/cpp/numeric/lcm.html "cpp/numeric/lcm")|

### Example

Run this code

#include <numeric>
 
int main()
{
    constexpr int p{2 * 2 * 3};
    constexpr int q{2 * 3 * 3};
    static_assert(2 * 3 == std::gcd(p, q));
 
    static_assert(std::gcd( 6,  10) == 2);
    static_assert(std::gcd( 6, -10) == 2);
    static_assert(std::gcd(-6, -10) == 2);
 
    static_assert(std::gcd( 24, 0) == 24);
    static_assert(std::gcd(-24, 0) == 24);
}

### See also