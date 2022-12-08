---
layout: post
title: 'Debug'
subtitle: 'Using VS code'
date: 2022-12-01
author: 'Jiali'
header-img: 'img/post-bg-idea.jpg'
tags:
  - VS code
  - Frontend
---

This is a note to record how to debug in VS code

## Debug node.js
To debug in VS code, you need to create `launch.json file`. In VS Code, you can select Run and Debug on the left (the fourth icon). Then click create a launch.json file and select Node.js. VS Code will create a template `launch.json` file for you. 

But you may need to change some parameters to make it work in different applications. Here are some notes from the [doc](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

> - program - an absolute path to the Node.js program to debug.
> - args - arguments passed to the program to debug. This attribute is of type array and expects individual arguments as array
elements. 
> - cwd - launch the program to debug in this directory.

The `launch.json` file saves in the .vscode folder in your directory

An example `launch.json` file would be like

```json
{
   "version":"0.2.0",
   "configurations":[
      {
         "type":"node",
         "request":"launch",
         "name":"Launch Program",
         "skipFiles":[
            "<node_internals>/**"
         ],
         "program":"${workspaceFolder}/app/server.js"
      }
   ]
}
```
```json
{
   "version":"0.2.0",
   "configurations":[
      {
         "type":"node-terminal",
         "name":"Run Script: debug",
         "request":"launch",
         "command":"npm run debug",
         "cwd":"${workspaceFolder}"
      }
   ]
}

```

## Run debug
The name in `launch.json` file will be the name shown in the Run and debug.  
You can Press `F5` or click `go running and Debug` in the left side panel and click the `Start debugging` button to start.  
You must stop other running servers before debugging if they run on the same port. Otherwise, you will get the error:  
> `Uncaught Error Error: listen EADDRINUSE: address already in use :::port number`

In the VS Code window, you can find `BREAKPOINTS` which contains all breakpoints you created. You can uncheck them if you don't want VsCode to stop at some breakpoints.  

The toolbox hover on the right side of the window gives you options to Continue, Step Over, Step Into, Step Out, and Stop.
- Continue will continue the program until the next breakpoint
- Step Over will run the next line code
- Step into will jump into the current methods if there are any
- Step-out will jump out of the current method if there are any
- Stop will stop debug  
  
### Variable and watch
In the variable box, you can check variables during debugging. To specifically check a variable, you can add it in the `WATCH`.  
. It will show not available before any action. When you are in debug mode, and the program stops at a breakpoint, it will show with values.  

## Debug Chrome
For react applications, we can also use VScode to debug. You need to create a `launch.json` file for Chrome. So instead of choosing Node.js, you need to choose Web App(Chrome)  

VS Code will provide you with a template `launch.json` file
```json
{
 // Use IntelliSense to learn about possible attributes.
 // Hover to view descriptions of existing attributes.
 // For more information, visit: https://go.microsoft.com/fwlink/? linkid=830387

   "version":"0.2.0",
   "configurations":[
      {
         "type":"chrome",
         "request":"launch",
         "name":"Launch Chrome against localhost",
         "url":"http://localhost:8080",
         "webRoot":"${workspaceFolder}"
      }
   ]
}
```

You need to replace the URL with your actual local server URL. In my case, it’s http://localhost:3000.
Before debugging, you need to start your server. Then launch the debug. A chrome window should pop up when debugging starts. The rest are
the same as debug Node.js.  


## Debug Jest
We can also take advantage of debugging in testing. We can add multiple configurations by adding in the configuration array as the below code shows.  

```json

 // Use IntelliSense to learn about possible attributes.
 // Hover to view descriptions of existing attributes.
 // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
{
   "version":"0.2.0",
   "configurations":[
      {
         "type":"chrome",
         "request":"launch",
         "name":"Launch Chrome against localhost",
         "url":"http://localhost:3000",
         "webRoot":"${workspaceFolder}"
      },
      {
         "type":"node",
         "request":"launch",
         "name":"Jest: current file",
         "program":"${workspaceFolder}/node_modules/.bin/jest",
         "console":"integratedTerminal",
         "disableOptimisticBPs":true,
         "windows":{
            "program":"${workspaceFolder}/node_modules/.bin/jest"
         }
      }
   ]
}

```

You only need to run `Jest: current` file to debug testing. This will run all tests in the application. If you only want to run a specific test, you can add args in the configuration. For example, I only want to debug EditMachine.test.jsx. So I add "args": "EditMachine.test.jsx" in the configuration. The rest are the same as debugging `Node.js` and `chrome`.  

```json
{
   "type":"node",
   "request":"launch",
   "name":"Jest: current file",
   "program":"${workspaceFolder}/node_modules/.bin/jest",
   "args":"EditMachine.test.jsx",
   "console":"integratedTerminal",
   "disableOptimisticBPs":true,
   "windows":{
      "program":"${workspaceFolder}/node_modules/.bin/jest"
   }
}

```

# Reference
- [VS code official doc]([Debugging](https://code.visualstudio.com/docs/editor/debugging))
- [Debugging Jest](https://gist.github.com/jherax/231b2dda7f9cce20e13f4590594fdb70)