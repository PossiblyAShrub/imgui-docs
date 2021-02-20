THIS IS WIP/DRAFT / [Discuss this article](https://github.com/ocornut/imgui/issues/3815)

## Why

**This is an attempt at explaining what the IMGUI paradigm stands for, and what it could be**.

Proponent of the IMGUI paradigm have noticed that it was widely misunderstood, over and over.
As of Feb 2021, even the [IMGUI Wikipedia page](https://en.wikipedia.org/wiki/Immediate_mode_GUI) is completely off the mark. There are reasons for that:
- the acronym is very misleading.
- people didn't do a good job enough explaining or documentation what IMGUI means?
- they are different interpretation of what IMGUI means?
- many popular IMGUI implementation have made similar design choices, making it more confusing what is actually the soul and backbone of the IMGUI paradigm vs a set of implementation choices. 
- the naming and success of "Dear ImGui" blurred the line a little bit further (should have used another name).

The acronym is very misleading because "immediate-mode" was initially coined as a reference to obsolete graphics API which made it very easy to render contents. The choice of that word got us into years, maybe decades of misunderstanding.

From now on, IMGUI will stand for "Incredibly Magic Graphics User Interface" ;)

The second purpose of this page should be to make it clear that there is a large space to explore in UI programming.

## History

The acronym IMGUI was coined by Casey Muratori in this video in 2005:
<BR>  https://www.youtube.com/watch?v=Z1qyvQsjK5Y (see also [blog](https://caseymuratori.com/blog_0001))

_"I’ve also seen lots of people getting into arguments about immediate-mode vs. retained-mode GUI APIs. I think this has something to do with how simple the IMGUI concept is, as it leads people to think they understand it, and then they proceed to get into heated arguments as if they actually know what they’re talking about. I rarely see this problem when I’m talking about anything even mildly complicated, like quaternions."_

It has been privately researched before and after.
Casey's work on formulating and popularizing research on this topic has been invaluable. I however believe that picking the word "immediate-mode" was a mistake which a decade later we are still paying for in term of misunderstanding what IMGUI are.

## Do we need a definition?

_"I kind of wish there was more radical experimentation in this space"_ [tweet](https://twitter.com/pervognsen/status/1361241939593416705)

Nowadays I am starting to believe that the term has caused more harm than benefits, because it suggests there are two camps "IMGUI" vs "RMGUI". The reality is there is a continuum of possibilities over multiple unrelated axises. Many of them haven't been explored enough, as popular IMGUI libraries have been designed for certain needs and not others. Many of them are at reach as extension of existing popular frameworks. Some are likely to only ever exist as part of yet undeveloped IMGUI frameworks.

The existence of those two terms effectively has hindered both discussions and research. The fact that we are even trying to put an official definition to a label is hindering people's imagination about the many possible areas of researches to be done in UI space.

## Half of a definition

@ocornut's attempt for a definition (WIP, 2021)

What we care about:

- IMGUI refers to the API: literally the interface between the application and the UI system.
  - The API tries to minimize the application having to retain data related to the UI system.
  - The API tries to minimize the UI system having to retain data related to the application.
- IMGUI does NOT refer to the implementation. Whatever happens inside the UI system doesn't matter.

This is in comparison with typical RMGUI ("retained-mode UI"):

- RMGUI APIs often have the application retain artifacts from the UI system (e.g. maintain references to UI objects).
- RMGUI APIs often require synchronization mechanisms because the UI system retain more application data.

What it doesn't stands for:

- IMGUI does not mean that the library doesn't retain data.
- IMGUI does not mean that stuff are drawn immediately.
- IMGUI does not mean it needs a continuous loop nor need to refresh continuously.
- IMGUI does not mean it needs to refresh all-or-nothing.
- IMGUI does not mean that states are polled.
- IMGUI does not mean the UI doesn't look "native" or looks a certain way.
- IMGUI does not mean it cannot animate.
- IMGUI does not mean it needs a GPU to render.
- IMGUI does not mean that the library can or cannot support feature x or y.
- IMGUI does not mean that the library is more or less portable.

TODO: Each of those points should be explained with a paragraph. We could also describe how common UI libraries (of all types) stand on a given axis. We could also describe less explored paths and envision what new UI libraries could do.

It's particularly important we develop this section to dissociate the promise (sometimes unfulfilled) vs current implementation. The full-feature bell-and-whistle promise is more likely to be ever fulfilled by a UI library if people understand that it is possible.

## Vurtun's write-up

[@vurtun](https://github.com/vurtun) said very eloquently around April 2019:

_"I usually don't like to write about immediate mode UI since there are a lot of preconceived notions about them. Addressing all of them is hard since often that is required since they all are thrown at once. For example a single implementation of a concept does not define a concept (dear imgui is a immediate mode UI not all imgui are like dear imgui)._

_Everything that can be done with "retained" gui can by definition be done by an immediate mode gui (I hate this whole divide at this point). Then we have state. For some reason like andrew already said I often see confusion that immediate mode means stateless. Immediate mode has nothing to do with how or if state is stored. Immediate mode in guis doesn't remove having two data representation. One on the calling side and one in the library. Even dear imgui and nuklear both have internal state they keep track of. Rather at its core immediate mode is about how this state is updated. In classical gui the state of the application was synchronized to the gui state by modification. On the other hand immediate mode is closer to functional programming. Instead of mutating state, all previous state is immutable and new state can only be generated by taking the previous state and applying new changes. Both in dear imgui and nuklear there is very little previous state and most is build up every "frame"._

_Next up handling widget events (press,clicked,...). Immediate mode has nothing to do with events vs polling for input changes. The famously convenient if (button(...)). You can even have retained libraries that use polling instead of events._

_Then we have "immediate mode bundles layouting, input and rendering". Once again only dear imgui and nuklear do this. At this point all my imguis have multiple passes. For example if I cache state for layouting I have (layout, input, render). Any change in the input or render pass will will jump back to layouting. If I don't cache state for layouting I still have two passes (input, render) to prevent glitches. To make sure that performance is not a problem I make sure that only those elements that are actually visible are updated, which is important for datastructures (list, trees). Invisible widgets are not only not rendered but also not layouted or have input handling. In a current implementation they are not even iterated over._

_Another argument I've read is that immediate mode cannot be used with OOP. While I try to stay away from that term since at this point it has taken so many meaning that it is basically worthless, what I hear it in context of is having composability for widgets. Once again just because dear imgui and nuklear don't have does not mean it has to be or even should be that way. For example the imgui we have at work supports composability and just like retain mode UIs you can put any widget inside any other. Want a slider inside a tab you can just compose them (not that it makes sense in this case)._

_Getting to game vs. tool ui. Disclaimer: I spend most of my time on tool ui however I played around with game ui or rather the concept behind it in my immediate/retain mode hybrid (https://gist.github.com/vurtun/c5b0374c27d2f5e9905bfbe7431d9dc0)._

_Personally for me tool and game ui are two completely different use cases. Interestingly at work we actually have a single immediate mode core that is used for both game as well as tool ui and it still works. However our advantage is that our UI designer can code. Immediate mode biggest advantage is that state mutation is extremely simple and easy. However in games all UI (including stuff like animations) outside datastructures likes list are know at compile time or loading time. So most advantages of immediate mode doesn't hold up (outside mentioned datastructures). Interestingly for datastructures you can even have an immediate mode API inside a retained state UI. So having the best of both worlds by having a hybrid._

_Finally state handling. For games this is easier since most state is known beforehand and can be reserved/preallocated. For tool ui it is a bit more complicated. First of it is possible to have state per widget. That is what we currently do at keen core. The state itself is lifetime tracked and freed if not used anymore. However what is actually possible as well is having a concept of an window/view and managing state outside the library. By that I mean a tabbed window with content. Like for example a log view or scene view storing all needed state for the UI and the actually data needed (log entries for the log view). Instead of trying to do state handling on the widget level it makes a lot more sense to have state management inside the view itself, since unlike the actually widgets the view itself has an easy to track lifetime. Just to clarify the view in the library API would just an handle while the actual view implementation would store the state._

_A lot in ui depends on the problem on hand so I feel there is no silver bullet. But what is actually great about UI is that it is possible to write a version fitting the problem. I hope if anything is taken from this rant is that "imgui" currently entangles a lot of concepts that however don't define it. So everytime it does not work out immediate mode as a grand concept itself is tossed aside instead of reevaluating the parts that did not work out."_

## Links

- [Johannes 'johno' Norneby's article](http://www.johno.se/book/imgui.html), 2007.
- [A presentation by Rickard Gustafsson and Johannes Algelind](http://www.cse.chalmers.se/edu/year/2011/course/TDA361/Advanced%20Computer%20Graphics/IMGUI.pdf), 2011.
- [Jari Komppa's tutorial on building an IMGUI library](http://iki.fi/sol/imgui/), @jarikomppa, 2006.
- [Casey Muratori's original video that popularized the concept](https://mollyrocket.com/861), 2005.
- [Nicolas Guillemot's CppCon'16 flash-talk about Dear ImGui](https://www.youtube.com/watch?v=LSRJ1jZq90k), 2016.
- [Thierry Excoffier's ZMV (Zero Memory Widget)](http://perso.univ-lyon1.fr/thierry.excoffier/ZMW/), 2004.
- [Unity's IMGUI](https://docs.unity3d.com/Manual/GUIScriptingGuide.html)
