# Integrating Dear ImGui in a New or Existing application

You should now be able to successfully build Dear ImGui. The next step is correctly integrating it with your app/game. This
involves the use of backends. For brevity we will not do in depth with backends in this article. However, if you are not sure
as to:

 - What backends are
 - How backends work
 - Which backends you should use

Then please read the [primer on backends](reference/backends).

First, in the file in which you will be handling Dear ImGui boilerplate; we must include `imgui.h` and the header files for your 
backend(s).

```cpp
#include <imgui.h>
#include <impl_imgui_XXXX.h> // One of these for each of your backend header files
```

!> If you encounter errors regarding your compiler being unable to find any of the above files please make sure you've
   properly configured your include directories as specified in the
   [building section](getting-started/building#building-the-source)

!> Also note, IDE's can easily get confused and emit false positive errors in regards to include directories. When in doubt
   attempt a build, and your compiler will complain if there's actually an error.


Integration in a typical existing application, should take <20 lines when using standard backends. Note the four key points in
which Dear ImGui boilerplate must occur:

**At initialization:**
 - call `ImGui::CreateContext()`
 - call `ImGui_ImplXXXX_Init()` for each backend.

**At the beginning of your frame:**
 - call `ImGui_ImplXXXX_NewFrame()` for each backend.
 - call `ImGui::NewFrame()`

**At the end of your frame:**
 - call `ImGui::Render()`
 - call `ImGui_ImplXXXX_RenderDrawData()` for your Renderer backend.

**At shutdown:**
 - call `ImGui_ImplXXXX_Shutdown()` for each backend.
 - call `ImGui::DestroyContext()`

For example, in a standard `while (running)` event loop:

```cpp
int main()
{
    // initialization

    while (running)
    {
        // beginning of frame

        // Dear ImGui API calls
        ImGui::ShowDemoWindow();

        // end of frame
    }

    // shutdown
}
```

For brevity we will not list the function signatures for every backend combination. You will have to find an
[example project](https://github.com/ocornut/imgui/tree/master/examples) which uses your combination of backends.
These projects contain the canonical usage of the standard backends, and are thoroughly commented.

## Dear ImGui Configuration Options

Right after calling `ImGui::CreateContext()` it is canonical to:

 - Enable applicable configuration flags
 - Set style options
 - Load fonts

To learn all about loading fonts please see the [font reference](reference/fonts).

#### Setting configuration flags

Configuration flags can be set like so:

```cpp
ImGuiIO& io = ImGui::GetIO();
io.ConfigFlags |= ImGuiConfigFlags_NavEnableKeyboard;     // Enable Keyboard Controls
io.ConfigFlags |= ImGuiConfigFlags_NavEnableGamepad;      // Enable Gamepad Controls
```

For a full list of config flags, see the [`ImGuiConfigFlags` enum](https://github.com/ocornut/imgui/blob/master/imgui.h#L1353).

#### Setting Style Options

Dear ImGui allows for plenty of style configuration. The easiest way to get started with styling is by using the built in style 
editor. This can be displayed by first showing the demo window, more on that in the [next section](). And then
**navigating to `Demo Window > Tools > Style Editor`**.

## Common Issues

It is easy to make mistakes, after all nobody is perfect. Below are the solutions to many common issues that arise while 
integrating Dear ImGui.

!> **Make sure to carefully compare your code to that of the [examples](https://github.com/ocornut/imgui/tree/master/examples).**

#### Q: I encounter linker errors or symbol not found/defined errors

**You have not properly built/linked Dear ImGui with you application.**

Please review the [building section](getting-started/building) and ensure you've followed *all* of the steps without error.

#### Q: My Application is not responding to user input

You are not calling *all* of the required input handling (new frame) functions.

Or, you have not correctly set up your windowing library.

**Make sure to carefully compare your code to that of the [examples](https://github.com/ocornut/imgui/tree/master/examples).**

#### Q: I do not see any UI rendering

There are two reasons as to why this maybe:

 1. You aren't actually calling any widget functions see the [basic usage](getting-started/usage) section for more.
 2. You aren't properly calling the render functions
 
**Make sure to carefully compare your code to that of the [examples](https://github.com/ocornut/imgui/tree/master/examples).**

#### Q: My fonts aren't appearing correctly

Font's are a large topic on their own. In the case of any errors regarding fonts please see the
[fonts documentation](reference/fonts).
