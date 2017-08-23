# Debugging NodeJS

You might remember from the front end foundations that you can place a `debugger;` statement anywhere in your JavaScript code, and the browser will stop at that line of code when the page loads so that you can inspect the state of your application.

You will be happy to know that you can continue to rely on `console.log` for revealing the inner workings of your code. One caveat: The results will appear in your terminal output, not in the Chrome DevTools console, so keep that in mind when you, out of habit, click on the console tab in your browser and see unhelpful blankness staring back at you.

But, for more control over the debugging process, such as adding breakpoints on specific lines of your JS code, inspecting the call stack, or to track variables before and after they are defined or changed, a more robust system is needed. Debugging server-side JavaScript with Chrome's DevTools is not as straight forward as client-side, but it can be done with a some configuration. The  `--inspect` [tag](https://medium.com/@paul_irish/debugging-node-js-nightlies-with-chrome-devtools-7c4a1b95ae27) can be added when launching your app, which will allow launching the DevTools to inspect your code pretty much in the same manner you're used to.

For another option, look at switching from Sublime Text to [Visual Studio Code](https://code.visualstudio.com/), an open source editor from Microsoft(!). It's a marvelous editor, with tons of built in features, including a customizeable debugger. With it, you can debug to your heart's content right in the editor itself. That takes the browser out of the equation, speeding up your work, since you now don't have to jump back and forth between the editor and Chrome or your terminal. Fewer keystrokes, FTW!
