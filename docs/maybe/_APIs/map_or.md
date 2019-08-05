**maybe&lt;T&gt;::map_or(T def, F f) -> T**

```cpp
template <class T>
class maybe {
  template <class U, class F>
  std::enable_if_t<
    std::conjunction_v<
      std::is_invocable<F&&, T&>,
      meta::has_type<std::common_type<U&&, std::invoke_result_t<F&&, T&>>>>,
  std::common_type_t<U&&, std::invoke_result_t<F&&, T&>>>
  map_or(U&& def, F&& f) & ;

  template <class U, class F>
  std::enable_if_t<
    std::conjunction_v<
      std::is_invocable<F&&, T const&>,
      meta::has_type<std::common_type<U&&, std::invoke_result_t<F&&, T const&>>>>,
  std::common_type_t<U&&, std::invoke_result_t<F&&, T const&>>>
  map_or(U&& def, F&& f) const& ;

  template <class U, class F>
  std::enable_if_t<
    std::conjunction_v<
      std::is_invocable<F&&, T&&>,
      meta::has_type<std::common_type<U&&, std::invoke_result_t<F&&, T&&>>>>,
  std::common_type_t<U&&, std::invoke_result_t<F&&, T&&>>>
  map_or(U&& def, F&& f) && ;
};
```

Applies a function to the contained value (if any), or returns the provided default (if not).

**Example**

```cpp
// begin example
#include <mitama/maybe/maybe.hpp>
#include <cassert>
#include <string>
using namespace mitama;
using namespace std::string_literals;

int main() {
  maybe x = just("foo"s);
  assert(x.map_or(42, &std::string::size) == 3);
  maybe<std::string> y = nothing;
  assert(y.map_or(42, &std::string::size) == 42);
}
// end example
```