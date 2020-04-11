# C#/C++ interop

## C# invoke C++ function

Sometimes, the functions implemented in C++ and we want to invoke the functions in C#. And we need to do some changes to enable the interop between C# and C++.

Before discussing the solutions, we discuss the several possible scenarios:
1. Be able to access the C++ source code
2. Only have .lib static library and header files

For #1, since you could able to access the source code and build it, you could define the export APIs and compile it as .dll dynamic link library. And then you could use the DllImport method in C# to call the API directly. An additional approach is using CLI, which is the thin wrapper. Please click [here](https://docs.microsoft.com/en-us/cpp/dotnet/how-to-wrap-native-class-for-use-by-csharp?view=vs-2019) to see an example.

For #2, we need to split it into several cases:
1. The (C) APIs already defined
2. APIs not defined, .lib is compiled as MT/MTD
3. APIs not defined, .lib is compiled as MD/MDD

In the first case, you could use the [module definition](https://docs.microsoft.com/en-us/cpp/build/reference/module-definition-dot-def-files?view=vs-2019) (.def) to export the APIs.

There are two solution to enable the c# invoke the function in c++: CLI solution and C++ export. The first solution is basically an intermediate layer for wrapping the functions in C++. The second solution is exporting the c++ function to dll and then use the dll import in c# to call the functions. 

### CLI solution

### C++ export

### Module definition (.def)
You could list the APIs in the .def file, and then link the .lib to generate the dll file.

Following is an example (say mydef.def):
```
EXPORTS
   function1
   function2
```

Then you could simply generate the dll by: `link /dll /def:mydef.def mylib.lib`

# Reference
- https://limbioliong.wordpress.com/2011/08/14/returning-an-array-of-strings-from-c-to-c-part-1/
- https://limbioliong.wordpress.com/2011/06/16/returning-strings-from-a-c-api/
- https://docs.microsoft.com/en-us/cpp/dotnet/how-to-wrap-native-class-for-use-by-csharp?view=vs-2019
- https://www.codeproject.com/Articles/28969/HowTo-Export-C-classes-from-a-DLL
