# CMake添加pthread链接

```css
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package (Threads)
...
add_executable(hp_server main.cpp)
...
target_link_libraries(hp_server ${CMAKE_THREAD_LIBS_INIT}) 
```

