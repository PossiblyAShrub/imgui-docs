# The basics of using Dear ImGui

You have now successfully, built and integrated Dear ImGui with your application. To ensure everything is working, let's
display the demo window.

## Showing the Demo Window

*Anywhere* between the `ImGui::NewFrame` and `ImGui::Render` functions call:

```cpp
ImGui::ShowDemoWindow();
```

This should display the demo window. If you encounter any errors, please see
[common issues](getting-started/integrating#common-issues).

The demo window **is incredibly helpful for both the developers and users of Dear ImGui**. This is because it acts as
an interactive guide and in-app documentation of the entire library. Also, all of the code is under `imgui_demo.cpp`, with 
plenty of comments to aid in understanding how the library is supposed to be used. **So please do not delete the file**, if 
your worried about compiled binary size, the linker will strip out the entire file if no calls to `ImGui::ShowDemoWindow` are 
made.

## Your first Dear ImGui Window

Now it's time to create your very first Dear ImGui window! Once again, *anywhere* between the `ImGui::NewFrame` and 
`ImGui::Render` functions call:

```cpp
if (ImGui::Begin("My First Window")) {
    ImGui::Text("Hello Dear ImGui!");
}
ImGui::End();
```

Dear ImGui windows begin via a call to `ImGui::Begin`, and end with a call to `ImGui::End`. The string in the first param of 
`ImGui::Begin` acts as the window's name, and it's id. The id must be unique in order to have different windows. If the ids are
the same, then the windows will concatenate.

!> Note: The call to `ImGui::End` *must* occur outside of the if statement

?> Dear ImGui uses id's to differentiate between unique widgets.
   [Read more about id's here](reference/faq#q-why-is-my-widget-not-reacting-when-i-click-on-it).

Any calls to widget functions occur within the if statement's body. Here we draw a Text widget, in order for it to always be 
visible, we must call it every frame. Because of this, we can do the following:

```cpp
static bool show_text = false;
ImGui::Checkbox("Show Text", &show_text);
if (show_text)
    ImGui::Text("Hello Dear ImGui!");
```

or

```cpp
for (int i = 0; i < 10; i++)
    ImGui::Text("[%04d] Hello Dear ImGui!", i);
```

Notice that with an immediate mode graphical user interface (IMGUI) there are no delete functions. To stop showing a widget
simply don't call it in the first place. To learn more about what the IMGUI paradigm even is see
[About the IMGUI paradigm](reference/imgui-paradigm).

That's all that you need to know to get started with Dear ImGui. **Remember to use the Demo Window as a reference for all of**
**Dear ImGui's provided widgets, and their usage.**
