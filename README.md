# Evo.Blazor.Canvases

A blazor library that makes it easier to write to the canvas when coding using blazor.

# Learning

I found it is not only useful to document and keep track of the final solution, but how one got there.  For the first couple of days working on this project I had to learn a lot about canvases and still have a long way to go.   To help others stumble less, and for me to be able to keep track of where I got information, here are the links I found useful:

- [WebGL Anti-Patterns](https://webglfundamentals.org/webgl/lessons/webgl-anti-patterns.html) - This link helped a ton.  It explained some important concepts and how to best resize the canvas and when window or other changes in the page cause the canvas to resize.  Thank you!

- [Resize Observer](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver) - Useful when learning to design an canvas that will resize appropriately when the window or elements change.  See also the [WebGL Anti-Patterns](https://webglfundamentals.org/webgl/lessons/webgl-anti-patterns.html) link above.

- [Blazor - Setting up a JSInterop library using TypeScript](https://www.williamtulloch.com/blazor;/.net/2018/09/30/Blazor-JSInteropWithTypeScript.html) - Explains how to setup a typescript library when building out Blazor components when wanting to include javascript in the pipeline.

- [Reszing Vector Graphics](https://medium.com/@doomgoober/resizing-canvas-vector-graphics-without-aliasing-7a1f9e684e4d)

- [Resize HTML5 Canvas to Fit Window](https://stackoverflow.com/questions/1664785/resize-html5-canvas-to-fit-window/63642064#63642064) - Good article referencing some the articles above.  Lots of good opinions.

- [Various Blazor Extensions with Javascript Dependencies](https://github.com/BlazorExtensions/) and the [Blazor Canvas Extension](https://github.com/BlazorExtensions/Canvas) - Was very useful when building early prototypes.  Then discovered I needed to fix a few architectural issues and thus the creation of this project.

- [Various Blazor Extensions without Javascript Dependencies](https://github.com/arivera12)

- [Blazor Components build by Microsoft](https://github.com/dotnet/aspnetcore/tree/d97be901b5e0917546a7aba4d52ada7862a058e0/src/Components) - The Web and WebJS folders contain blazor components.  

- [Blazor WebAssembly 3.2.0 Preview 2 Release Notes](https://devblogs.microsoft.com/aspnet/blazor-webassembly-3-2-0-preview-2-release-now-available/) - Mentions that BlazorLinkOnBuild in your project files needs to be changed to BlazorWebAssemblyEnableLinking

- [NPM won't install Dev Dependencies](https://stackoverflow.com/questions/34700610/npm-install-wont-install-devdependencies) and [Difference Between --save and --save-dev](https://stackoverflow.com/questions/22891211/what-is-the-difference-between-save-and-save-dev)- Was useful when learning the solution pattern that includes using a *.JS project generate javascript libraries from typescript.



- [Compile Typescript Code with NPM](https://docs.microsoft.com/en-us/visualstudio/javascript/compile-typescript-code-npm?view=vs-2019) - Useful if trying to learn about how webpack and npm work together to build typescript code.

- [Microsoft Blazor Documentation](https://docs.microsoft.com/en-us/aspnet/core/blazor/?view=aspnetcore-5.0) and [Blazor Component Class Libraries](https://docs.microsoft.com/en-us/aspnet/core/blazor/components/class-libraries?view=aspnetcore-5.0&tabs=visual-studio)- Just useful to have handy.

- [MelonJS](https://github.com/melonjs/melonJS/) - Open source game engine that was useful to reference

- [Html5 canvas crisp lines every time](https://mobtowers.wordpress.com/2013/04/15/html5-canvas-crisp-lines-every-time/)

## Blazor

- [.NET 6 ASP.NET Core and Blazor Roadmap](https://github.com/dotnet/aspnetcore/issues/27883)

## JS Interop Speed / Performance
- [Using IJSUnmarshalledRuntime - Example 1](https://www.meziantou.net/generating-and-downloading-a-file-in-a-blazor-webassembly-application.htm)
- [Canvas Filled Three Ways - JS, WebAssembly, WebGL](https://compile.fi/canvas-filled-three-ways-js-webassembly-and-webgl/)

## Javascript

- [Better Way to Add Dynamic Methods](https://stackoverflow.com/questions/584907/javascript-better-way-to-add-dynamic-methods)
- [Creating Functions Dynamically with Javascript](https://www.thatsoftwaredude.com/content/9025/creating-functions-dynamically-with-javascript)
- [Javascript 'new Function(...)' syntax](https://javascript.info/new-function)

# Architecture

## Library Size

I got asked a I learned over the last year it is important to make projects very small, as the amount of time available to work on projects truly dictates whether a project can be finished or not - and finishing a project is probably the number one thing to be able to do - else the work one does can be chaulked up to just being a technical exercise / a game.

## Namespace

The namespace starts with Evo.  This is important for two reasons. One, when using other packages that use Blazor as a parent namespace, it is hard without using the global namespace to fix namespace conflict compile time errors.  Second, it specifies from which organization this came from, which is again helps limit namespace conflicts.  

## Canvas Resizing Approach

Resizing the canvas appropriately and efficiently is not as straight forward as it seems, especially for developers who are not experts in HTML5 graphics.  Thus, it was important to research and identify solutions others were using.  It was found that others struggle with the same issues and that there were a number of good articles discussing how to best resize the canvas as window and controls within the page changed size.  Those articles are listed immediatley below in the references section specific to this topic. 

### Handling it When Rendering (slow)

The first approach attempted was to check to see if the canvas needed to be resized each time it was drawn to the screen.  Upfront it was known this was going to be a slow and inefficient approach, as the gut said go css or something like that, but that required research and the goal was to just get the damn thing working first.  Well, it worked to some degree -- definalty killed performance though.  

It was found out while using this appraoch that resizing the canvas clears the canavas - and that this fact surprises many.  To optimize drawing operations, ideally only the porition of the canvas that needs to be redrawn should be; thus resizing needs to be removed from the rendering pipeline unless it is needed.  

### Did some Research, Back to CSS

Frustrated, it was time to do some more research and identity approaches working for others.  Eventually, jackpot was hit: [WebGL Anti-Patterns](https://webglfundamentals.org/webgl/lessons/webgl-anti-patterns.html).   Looking at the first resize example, it shows the canvas is able to be resized by 

The current BECanvas takes in two numbers for its width and height.  Instead, I want the width and height to be set using css by specifying the width should be 100% the view ports and 

```
canvas {
    display: block;
    width: 100vw;
    height: 100vh;
}
```
Note, 'vw' and 'vh' suffixes stand for viewport width and viewport height, respectively.

The difference between these and using width being set to 100% is that while ```width: 100%``` will make the element fit all the space available, the viewport width has a specific measure, in this case the width of the available screen, including the document margin.  But if the style of the body is set to ```margin: 0;``` using ```100vw``` should behave the same as setting ```width: 100%```.

### References

# Objectives

The first objective of this project is to figure out how to draw a grid to the HTML5 canvas and for the grid to resize.  

# Examples

## Resizing 

[Example 01](https://github.com/Evobolics/Evo.Blazor.Canvases/tree/main/Examples/01/src) - Minimal Canvas Resizing without Scrollbars without using overflow: none;

## Producing a Grid

## Creating a Tile Map
