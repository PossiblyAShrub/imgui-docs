> Notice of archival. These docs have not been updated in years, I have decided to archive the project as it is no longer up-to-date with Dear ImGui.

# Dear ImGui Documentation Site

This is an attempt at creating higher level documentation for the wonderful
Dear ImGui library. [Demo here.](https://possiblyashrub.github.io/imgui-docs/#/)

I've also started working on a getting started guide, however it is still
very much a WIP.

The documentation is all written in markdown. I am using a tool
called [docsify](https://docsify.js.org/#/) which is a lightweight
documentation generator, and runs entirely on the client side.
(There is no 'build docs' step)

It can be hosted with any static hosting tool pointing to the root directory.
For example, there is a [live demo](https://possiblyashrub.github.io/imgui-docs/#/)
hosted with github pages.

Locally one can use any http(s) static server tools for development.
See a list [oneliners using common tools here](https://gist.github.com/willurd/5720255)

For example, if you have python 3 installed:
```
git clone https://github.com/PossiblyAShrub/imgui-docs
cd imgui-docs
python -m http.server 3000
```
And then visit http://localhost:3000.

Because the docs are just markdown files, it can be viewed in any markdown viewer.

Also! If you visit the documentation (via the site, or a local http server)
using any modern web browser (service workers must be enabled) you can view
the docs offline!
*Try visiting the site after disabling you connection/stopping your local server*

