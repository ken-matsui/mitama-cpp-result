**maybe&lt;T&gt;::ok_or_else(F f) -> result&lt;T, E&gt;**
**where F: () -> E**

```cpp
template <class T>
class maybe {
  template <class F>
  std::enable_if_t<
    std::is_invocable_v<F&&>,
  result<T, std::invoke_result_t<F&&>>>
  maybe<T>::ok_or_else(F&& err) const& ;

  template <class F>
  std::enable_if_t<
    std::is_invocable_v<F&&>,
  result<std::remove_reference_t<T>, std::invoke_result_t<F&&>>>
  maybe<T>::ok_or_else(F&& err) && ;
};
```

Transforms the `maybe<T>` into a `result<T, E>`, mapping `just(v)` to `success(v)` and `nothing` to `failure(err())`.

**Example**

```cpp
// begin example
#include <mitama/maybe/maybe.hpp>
#include <mitama/result/result.hpp>
#include <cassert>
#include <string>
using namespace mitama;
using namespace std::string_literals;

int main() {
  maybe x = just("foo"s);
  assert(x.ok_or_else([]{ return 0; }) == success("foo"s));

  maybe<std::string> y = nothing;
  assert(y.ok_or_else([]{ return 0; }) == failure(0));
}
// end example
```