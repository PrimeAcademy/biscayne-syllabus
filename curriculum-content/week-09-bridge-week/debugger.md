## Controlling A Debugger

Most debuggers behave the same and have similar interfaces. These terms and UI buttons will probably be available in any debugger you use.

## Objects

- Learn the **mechanics of using the Chrome debugger**


## Debugger UI

Chrome DevTools/Debugger is exposed from the Sources tab of the Developer Tools pane. Debug UI is in the middle and right-hand panes, with script source code in the left.

![](../images/debugger-ui.png)


## Common Debugger Features

### Breakpoints
Place breakpoints at places you want to inspect something, usually variable values coming into or leaving a function. When the app hits the line you have set as a breakpoint, it will pause, allowing you to examine (and sometimes change) values in order to see what is going on at that moment. 

**The line you pause at has NOT YET BEEN RUN.**

### Locals/Variables
Shows a list of local variables in the current scope. JS debugging also shows the stack of Closures and their variables.

### Watch/Watches/Watch List
Placing a variable in the Watch List allows you to see it’s value even when the variable is no longer in scope. Consider it “pinned” here.

### Call Stack
Shows the list of functions/closures called in order, usually with what parameter values were passed in.


## Controlling Code Execution

![](../images/debugger-ui-buttons.png)

### Play/Pause/Resume
Allows the program to continue to the next breakpoint as normal.

### Step Over
Executes the current line and then stops at next line.

### Step Into
Used on a function call to follow the flow into the function. Otherwise stepping over a function call will execute the function without stepping through whatever it does.

### Step Out
Completes the execution of any function or closure scope you’re currently in, moving you “out” of it to the next line of code following its completion/invocation.


## VSCODE (Server Side Debugging)

This is for debugging node based (server) code. VSCode comes with presets built in!

### SETUP
- Click the crossed out bug to open the debug pane
- The first time you do so in a project, there should be a gear with an orange circle on it
- In the dropdown, select node
- It will auto-generate a LAUNCH config.
- You need to change "program" key to the path to your server file: e.g: `"${workspaceFolder}/server/server.js"`

### Attach vs Launch

Launch will start your app AND attach a debugger to that instance. You have to point it to your server file. You also have to shut down your server before launching it.

Attach will connect a debugger to your running app. You have to run your app with --inspect `node --inspect server/server.js` and add protocol inspect in the launch.json attach.

```js
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "program": "${workspaceFolder}/server/server.js"
    }
```
