# Evo.Blazor.Canvases

A blazor library that makes it easier to write to the canvas when coding using blazor.

# Learning

I found it is not only useful to document and keep track of the final solution, but how one got there.  For the first couple of days working on this project I had to learn a lot about canvases and still have a long way to go.   To help others stumble less, and for me to be able to keep track of where I got information, here are the links I found useful:

- [WebGL Anti-Patterns](https://webglfundamentals.org/webgl/lessons/webgl-anti-patterns.html) - This link helped a ton.  It explained some important concepts and how to best resize the canvas and when window or other changes in the page cause the canvas to resize.  Thank you!

- [Resize Observer](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver) - Useful when learning to design an canvas that will resize appropriately when the window or elements change.  See also the [WebGL Anti-Patterns](https://webglfundamentals.org/webgl/lessons/webgl-anti-patterns.html) link above.

- [Blazor - Setting up a JSInterop library using TypeScript](https://www.williamtulloch.com/blazor;/.net/2018/09/30/Blazor-JSInteropWithTypeScript.html) - Explains how to setup a typescript library when building out Blazor components when wanting to include javascript in the pipeline.

- [Reszing Vector Graphics](https://medium.com/@doomgoober/resizing-canvas-vector-graphics-without-aliasing-7a1f9e684e4d)

- [Resize HTML5 Canvas to Fit Window](https://stackoverflow.com/questions/1664785/resize-html5-canvas-to-fit-window/63642064#63642064) - Good article referencing some the articles above.  Lots of good opinions.

- [Various Blazor Extensions without Javascript Dependencies](https://github.com/arivera12)

- [Blazor Components build by Microsoft](https://github.com/dotnet/aspnetcore/tree/d97be901b5e0917546a7aba4d52ada7862a058e0/src/Components) - The Web and WebJS folders contain blazor components.  

- [Blazor WebAssembly 3.2.0 Preview 2 Release Notes](https://devblogs.microsoft.com/aspnet/blazor-webassembly-3-2-0-preview-2-release-now-available/) - Mentions that BlazorLinkOnBuild in your project files needs to be changed to BlazorWebAssemblyEnableLinking

- [NPM won't install Dev Dependencies](https://stackoverflow.com/questions/34700610/npm-install-wont-install-devdependencies) and [Difference Between --save and --save-dev?](https://stackoverflow.com/questions/22891211/what-is-the-difference-between-save-and-save-dev)- Was useful when learning the solution pattern that includes using a *.JS project generate javascript libraries from typescript.

# Architecture

## Library Size

I got asked a I learned over the last year it is important to make projects very small, as the amount of time available to work on projects truly dictates whether a project can be finished or not - and finishing a project is probably the number one thing to be able to do - else the work one does can be chaulked up to just being a technical exercise / a game.

## Namespace

The namespace starts with Evo.  This is important for two reasons. One, when using other packages that use Blazor as a parent namespace, it is hard without using the global namespace to fix namespace conflict compile time errors.  Second, it specifies from which organization this came from, which is again helps limit namespace conflicts.  


