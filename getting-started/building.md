# Obtaining and Building the Dear ImGui Source

**The core of Dear ImGui is self-contained within a few platform-agnostic files** which you can easily compile in your
application/engine. They are all the files in the root folder of the repository
 - imgui.h
 - imgui_internal.h
 - imgui.cpp
 - imgui_demo.cpp
 - imgui_draw.cpp
 - imgui_tables.cpp
 - imgui_widgets.cpp

And, you will need to compile the applicable backend files.
(Located in [backends/imgui_impl_XXX.cpp](https://github.com/ocornut/imgui/tree/master/backends))
**You can read all about [backends here](/reference/backends)**.

## Obtaining the Source Code

The first part of building Dear ImGui is actually getting the source code. There are a plethora of ways to manage dependencies.
If you're brand new to managing C++ dependencies, using git submodules or vcpkg is highly recommended. However, Dear ImGui is
independent of build systems and package managers; if you already have your preferred dependency manager, feel free to use that
tool instead.

<!-- tabs:start -->

### **Git Submodules**

```bash
 $ git submodule add https://github.com/ocornut/imgui
```

### **Vcpkg**

```batch
 > .\vcpkg\vcpkg install imgui
 > .\vcpkg\vcpkg integrate install
```

### **CMake**

 * CMake version \>3.3

```cmake
find_package(imgui REQUIRED)
...
target_link_libraries(<my-target> ... imgui ...)
```

 * CMake version 2.8.12 - 3.2

```cmake
find_package(imgui REQUIRED)
...
add_exutable(<my-target> ... ${IMGUI_SOURCES})
target_link_libraries(<my-target> ... imgui ...)
```

<!-- tabs:end -->

## A Note about Backends

Dear ImGui is platform independent. However, in order to provide Dear ImGui with the necessary inputs/outputs, you will have to
provide platform dependant functions. These are called backends, which are required to integrate Dear ImGui in your app. The
backend passes mouse/keyboard/gamepad inputs and variety of settings to Dear ImGui, and is in charge of rendering the
resulting vertices.

**Backends for a variety of graphics api and rendering platforms** are provided in the
[backends/](https://github.com/ocornut/imgui/tree/master/backends) folder, along with example applications in the
[examples/](https://github.com/ocornut/imgui/tree/master/examples) folder. See the section on
[Integration](getting-started/integrating) for details. You may also create your own backend, if necessary. However, this is
only recommended for advanced users.

?> This is a very high level overview of backends. For the curious, see the
   [backend reference](reference/backends) - which is much more in depth.

**Each backend contains a `.cpp` file that will need to be built alongside Dear ImGui.**

## Building the Source

**No specific build process is required**. You can add the .cpp files to your existing project. Or you can build your project
based off of the numerous prebuilt [example/starter projects](https://github.com/ocornut/imgui/tree/master/examples).

However, to accommodate those who are new to C++, we've included step-by-step instructions for several common build systems.

*If you are already comfortable compiling 3rd party libraries feel free to skip to the [next section]()*


<!-- tabs:start -->

### ** Visual Studio **

TODO

### ** CMake **

TODO

### ** Premake **

TODO

### ** Make **

TODO

<!-- tabs:end -->

Congratulations! You should now be able to successfully build Dear ImGui. If you encounter any errors throughout this process,
please make sure you've followed every step exactly as described. And if you still encounter errors, please ask a
[thorough question](https://bit.ly/3nwRnx1) in the [discord server](http://discord.dearimgui.org/) or
[issue tracker](https://github.com/ocornut/imgui/issues).
