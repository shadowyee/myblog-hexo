---
title: Vscode配置c/c++环境
---
## Vscode配置c/c++环境

--start from 2024.1.1

---

* [Vscode docs](https://code.visualstudio.com/docs/cpp/introvideos-cpp)

> 1. 按```ctrl+e```呼出命令菜单，或者按```F1```
>
> 2. 输入```>edit configurations```$\rightarrow$使用UI界面进行配置$\rightarrow$生成```./.vscode/c_cpp_properties.json```
> 3. 点击```调试C/C++文件```右边的```添加调试配置```按钮$\rightarrow$生成```./.vscode/task.json```，可以设置编译参数字段```args```
> 4. 点击```调试C/C++文件```生成```./.vscode/launch.json```

* [gcc编译报错：undefined reference to std::cout](https://blog.csdn.net/weixin_41010198/article/details/117523288)

  > 使用g++编译器就行，但是为什么呢？

* [What is the difference between clang (and LLVM) and gcc / g++?](https://stackoverflow.com/questions/24836183/what-is-the-difference-between-clang-and-llvm-and-gcc-g)