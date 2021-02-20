(work in progress)

## General Terms

##### Backends/Bindings
A piece of code that connects your OS/platform/windowing system, graphics API or programming language to Dear ImGui. In the `examples/` folder we provide a bunch of `imgui_impl_xxxx` files which are backends for a variety of platforms and graphics API. Many other [third-party bindings](https://github.com/ocornut/imgui/wiki/Bindings) exist.

##### Demo
The demo code in `imgui_demo.cpp`, whose main entry point is the `ImGui::ShowDemoWindow()` function.

##### Docking
Refers to the feature (currently available in the `docking` branch) where multiple windows can be combined with each other, used to create tab bars, or laid out next to each other.

##### Draw command, ImDrawCmd
Refers to one item within an `ImDrawList`. One draw command generally maps to one draw call in the underlying graphics API. Some draw commands hold callbacks or special functions that don't necessarily map to an actual draw call in the graphics API.

##### Draw list, ImDrawList
Refers to the low-level rendering API which you can use to submit shapes (such as rectangles, lines, circles or text elements). The ImGui API uses the ImDrawList API to draw elements. You can easily access the drawlist of the current ImGui window using `ImGui::GetWindowDrawList()` or access special drawlists such as `ImGui::GetBackgroundDrawList()` (drawn before all ImGui windows) or `ImGui::GetForegroundDrawList()` (drawn after all ImGui windows). A draw list is generally comprised of multiple draw commands.

##### Examples
The example applications in the sub-folders of `examples/`. An example application generally combines one or two backends + dear imgui code + a main.cpp file to form a running application.

##### Font atlas
A font atlas manage multiple fonts, which for performance reasons are generally rasterized and packed together into a single graphics texture. The font atlas also contains other graphics data that is used by ImGui and ImDrawList.

##### Item (same as Widget)
A single ImGui element (e.g. a `ImGui::Button()` or `ImGui::Text()` call).

##### Navigation
Refer to the subsystem in charge of allowing full control of the UI using Gamepad or Keyboard inputs. Initially Dear ImGui mostly used mouse inputs, and the "navigation" feature allows directional navigation within a set of widgets.

##### Metrics
Refers to the confusingly named `Dear ImGui Metrics` window, whose main entry point is the `ImGui::ShowMetricsWindow()` function. The Metrics window exposes lots of internal information about the state of your Dear ImGui context. Using it can help you debug and understand many problems you may have with Dear ImGui. It may also help you better understand how Dear ImGui works.

##### Multi-Viewports
Refers to the feature (currently available in the `docking` branch) where Dear ImGui windows can easily be moved outside the boundaries of your main application window. The multi-viewports feature provides an interface to interact with bindings in order to create its own application windows to host the floating Dear ImGui windows.

##### Root Window
A top-level window created with `Begin()`. Child windows created with `BeginChild()` tend to share the same Root Window.

##### Widget: (same as Item)
A single ImGui element (e.g. a `ImGui::Button()` or `ImGui::Text()` call).

## Docking Terms

![Docking](https://user-images.githubusercontent.com/8225057/97541627-c0dea300-19c5-11eb-9416-8bb255e189a1.png)

##### Docking (noun)
Refers to the Docking subsystem as a whole.

##### Docking (verb)
Refers to the action of combining one Window or Dock Node into another Window or Dock Node. A docking operation can be a "Split" (split the target window/node into two sections) or an "Merge" (add into the existing target window/node). 

##### Docking Decorations
Tab Bar and Close Buttons (when managed by a dock node), Resizing Separators.

##### Dock Node
Carries a Tab Bar + zero or more Windows _or_ child dock nodes. A Dock Node can be either a Floating Dock Node or a Dockspace. 

##### Dock Node Hierarchy
A dock node and all its child dock nodes.

##### Dockspace
A dock node whose Host Window has been created by the user. This implies that the position and size of the dock node is not controlled by the Docking system. It also implies that the lifetime of the dock node is not controlled by the Docking system.

##### Floating Dock Node
A dock node whose Host Window is automatically created and managed by the Docking system. They are generally freely moveable.

##### Root Dock Node
A dock node that has no parent dock node.

##### Leaf Dock Node
A dock node that that no child dock nodes.

##### Host Window
The Window used to display _Docking Decorations_. In a Dock Node Hierarchy sitting over a Dockspace, the Host Window is always the window where the Dockspace was submitted. In a Dock Node Hierarchy sitting over a Floating Window, the Host Window is created by the Docking system.

## Multi-Viewports Terms

![MultiViewports](https://user-images.githubusercontent.com/8225057/97542423-fe8ffb80-19c6-11eb-9bf5-e26d86364e55.png)

Note: when talking about issues related to the multi-viewports feature, always try to remove ambiguity by specifying if an item is controlled by Dear ImGui or the host system - e.g. "_Dear ImGui Window_" vs "_Platform Window_".

##### Desktop
The area managed by the Operating System or Window Manager that covers all _Platform Monitors_.

##### Platform
Refers to the low-level system used to interface with native windows: it could be an Operating System library (Win32) or a cross-platform library (such as SDL, GLFW, Allegro).

##### Platform Backend
Refers to the piece of glue code used to have Dear ImGui interact with a given platform (e.g. the `imgui_impl_glfw.cpp` file). This code typically creates and destroy _Platform Windows_ associated with each _Viewport_, and sets up the _Platform Decorations_ based on the _Viewport_ flags.

##### Platform Decoration
Refers to the frame and title bar managed and displayed by the Operating System or Window Manager.

##### Platform Monitor
Refers to the representation of a monitor. On a modern OS, each monitor is associated with a non-overlapping set of coordinates (position, size), a work area (main area minus OS task bar) and a DPI scaling factor. Dear ImGui takes advantage of _Platform Monitor_ knowledge for certain tasks (for example, tooltip windows get clamped to fit within a single _Platform Monitor_ and not straddle multiple monitors, new popups try not to overlap the OS task-bar).

##### Platform Window
A window managed by the Operating System. This is typically associated with a framebuffer, a swap chain and the machinery to perform 3D rendering via some graphics API.

##### ImGui Decoration
Refers to the frame, title bar, collapse and close buttons managed and displayed by _ImGui Windows_.

##### ImGui Window
A window managed by Dear ImGui. For the purposes of multi-viewports, we often use "ImGui Window" to refer to a top-level _ImGui Window_ only, not child or docked windows.

##### Host Viewport
A _Host Viewport_ can contains multiple _ImGui Windows_. Currently the _Main Viewport_ is always a _Host Viewport_. In the future we would like to allow application to create zero, one or more _Host Viewports_. When a _Host Viewport_ is moved, all the _ImGui Windows_ it contains get moved along with it. By default, any _ImGui Window_ overlapping a _Host Viewport_ will be merged into it. Setting `io.ConfigViewportsNoAutoMerge` disables that behavior, causing every _ImGui Window_ to be hosted by their own unique _Single-Window Viewport_.

##### Single-Window Viewport, Owned Viewport
a _Viewport_ owned by a single _ImGui Window_. In the typical setup, dragging a _ImGui Window_ outside the boundaries of a _Host Viewport_ will create a _Single-Window Viewport_ for the purpose of allowing the _ImGui Window_ to be displayed anywhere on the _Desktop_.

##### Main Viewport
Due to common usage and backward compatibility reasons, the system currently enforces the creation of one _Main Viewport_ which is typically the main application window of the user application. The _Main Viewport_ is also a _Host Viewport_. In most traditional applications, the _Main Viewport_ has _Platform Decorations_ enabled when not maximized, and has _Platform Decorations_ disabled when fullscreen. In the future we would like to completely remove the concept of a "Main Viewport" and instead allow the application to freely create zero, one or more _Host Viewports_. Most games applications will create one of them, whereas most desktop applications are expected to create none. Under this scheme, an application creating no _Host Viewport_ would have every _ImGui Window_ using an individual _Platform Window_ with _Platform Decorations_ disabled. It would become the application's responsibility to shutdown when all the windows are closed.

##### Viewport
The Dear ImGui representation of a _Platform Window_. When a _Viewport_ is active, the _Platform Backend_ creates a _Platform Window_ for it. Whether the _Platform Window_ has _Platform Decorations_ enabled currently depends on `io.ConfigViewportsNoDecoration` flag which gets turned into a `ImGuiViewportFlags_NoDecoration` for the _Platform Backend_ to honor.
