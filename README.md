# Wrapping [reSolve](https://github.com/reimagined/resolve) project to [Electron](https://electronjs.org/) desktop application

[Electron](https://electronjs.org/) is a framework for creating native applications with web technologies like JavaScript, HTML, and CSS.
In this small tutorial we create [reSolve](https://github.com/reimagined/resolve) application and run it within [Electron](https://electronjs.org/) framework.

### Creating new [reSolve](https://github.com/reimagined/resolve) application

The simplest way to create [reSolve](https://github.com/reimagined/resolve) application is to use CLI named `create-resolve-app`.

```bash
npm install -g create-resolve-app
```

Create new application from example:

```bash
max$ create-resolve-app resolve-electron --example todo-two-levels
```

### Importing [reSolve](https://github.com/reimagined/resolve) application to [Electron](https://electronjs.org/) environment

[Electron](https://electronjs.org/) provides various tools for initializing new applications or importing/converting existing projects. We use one of the popular CLI tools for [Electron](https://electronjs.org/) - [Electron Forge](https://electronforge.io/).

```bash
npm install -g electron-forge
``` 

[Electron Forge](https://electronforge.io/) have `import` to wrap existing projects with [Electron](https://electronjs.org/) framework. 

```bash
electron-forge import resolve-electron
```

Its interactive command, but we need to override only `main` key in `package.json` to `electron/index.js`:

```bash
? Do you want us to change the "main" attribute of your package.json?  If you are currently using babel and pointing to a "build" directory say yes. Yes
? Enter the relative path to your uncompiled main file electron/index.js
``` 

### Create [Electron](https://electronjs.org/) entry point

Unfortunately [Electron Forge](https://electronforge.io/) does not add new [Electron](https://electronjs.org/) index file during import command. So we need to create one. This file can be copied from new [Electron Forge](https://electronforge.io/) project created with `init` command. For now you can create this file manually as *electron/index.js* (create *electron* directory first).
Please notice, that `mainWindow.loadUrl` method argument should load URL *http://localhost:3000* where running [reSolve](https://github.com/reimagined/resolve) development server. 

```js
import { app, BrowserWindow } from 'electron';

if (require('electron-squirrel-startup')) { // eslint-disable-line global-require
    app.quit();
}

let mainWindow;

const createWindow = () => {
    // Create the browser window.
    mainWindow = new BrowserWindow({
        width: 800,
        height: 600,
    });

    // Load reSolve web application
    mainWindow.loadURL(`http://localhost:3000`);

    mainWindow.webContents.openDevTools();

    mainWindow.on('closed', () => {
        mainWindow = null;
    });
};

app.on('ready', createWindow);

app.on('window-all-closed', () => {
    if (process.platform !== 'darwin') {
        app.quit();
    }
});

app.on('activate', () => {
    if (mainWindow === null) {
        createWindow();
    }
});

```

### Ready to launch

Now everything is ready. Firstly start [reSolve](https://github.com/reimagined/resolve) development server with

```bash
npm run dev
``` 
This command should run local backend server and open browser page pointed to *http://localhost:3000*

![Screenshot 01](https://github.com/max-vasin/resolve-electron-app/blob/master/screenshots/s_01.png)

Build and run [Electron](https://electronjs.org/) application

```bash
electron-forge start
```

![Screenshot 02](https://github.com/max-vasin/resolve-electron-app/blob/master/screenshots/s_02.png)

Try to add ToDo lists and items with both browser and desktop application to test reactivity provided by [reSolve](https://github.com/reimagined/resolve) app.

![Screenshot 03](https://github.com/max-vasin/resolve-electron-app/blob/master/screenshots/s_03.png)

![Screenshot 04](https://github.com/max-vasin/resolve-electron-app/blob/master/screenshots/s_04.png)


