# windows的visual studio检测内存泄漏

* 开发环境使用 [Visual Studio](https://visualstudio.microsoft.com/zh-hans/?rr=https%3A%2F%2Fcn.bing.com%2F)
* 在程序结束点调用 [\_CrtDumpMemoryLeaks\(\)](https://docs.microsoft.com/zh-cn/previous-versions/d41t22sb%28v=vs.120%29?redirectedfrom=MSDN)

```cpp
#include <crtdbg.h>

#ifdef _DEBUG
#define new new (_NORMAL_BLOCK, __FILE__, __LINE__)
#endif

int main() {
  int* p = new int(42);
  _CrtDumpMemoryLeaks();
}
```

* 调试时将提示第 9 行产生 4 字节的内存泄漏

```cpp
Detected memory leaks!
Dumping objects ->
xxx.cpp(9) : {88} normal block at 0x008C92D0, 4 bytes long.
 Data: <*   > 2A 00 00 00 
Object dump complete.
```

* 这种方法的原理是，在执行此函数时，检查所有未回收的内存，因此存在析构函数还未执行而误报的情况

```cpp
#include <crtdbg.h>

#include <memory>

#ifdef _DEBUG
#define new new (_NORMAL_BLOCK, __FILE__, __LINE__)
#endif

int main() {
  auto p = std::make_shared<int>(42);
  _CrtDumpMemoryLeaks();  // 此时 std::shared_ptr 还未析构，因此报告内存泄漏
}
```

* 更好的方法是在开始点使用 [\_CrtSetDbgFlag\(\)](https://docs.microsoft.com/zh-cn/previous-versions/5at7yxcs%28v=vs.120%29?redirectedfrom=MSDN)，它在程序退出时调用 [\_CrtDumpMemoryLeaks\(\)](https://docs.microsoft.com/zh-cn/previous-versions/d41t22sb%28v=vs.120%29?redirectedfrom=MSDN)

```cpp
#include <crtdbg.h>

#ifdef _DEBUG
#define new new (_NORMAL_BLOCK, __FILE__, __LINE__)
#endif

int main() {
  _CrtSetDbgFlag(_CRTDBG_LEAK_CHECK_DF | _CrtSetDbgFlag(_CRTDBG_REPORT_FLAG));
  int* p = new int(42);
}
```

* 该项目中的源码使用上述方式检测均无内存泄漏
* 为了尽可能简单，所有代码均以单个源文件的形式实现
* 源码中未使用平台相关 API，因此在任何支持 C++17 标准的平台均可通过编译

