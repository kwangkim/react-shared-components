# react-sharing-components

The concept executed is 99.9% inspired and based on [Sharing components between React and React Native](https://medium.com/@aakashns/sharing-components-between-react-and-react-native-f6ce3713658a) and [A mobile, desktop and website App with the same code](http://blog.benoitvallon.com/projects/a-mobile-desktop-and-website-app-with-the-same-code/)

Running the code
----------------

Make sure you have [Node JS](https://nodejs.org/en/) 4.0.0 or higher installed.

## Install

    npm install -g webpack-dev-server
    npm install -g react-native react-native-cli
    npm install

## Web

To run the web example, first run the following commands :

    npm run start-web

Now navigate to [http://localhost:8080/](http://localhost:8080/)

## Android

To run Android, first start an android emulator (or connect your phone with [USB debugging enabled](http://developer.android.com/tools/device.html)), then run the following commands :

    react-native run-android
    npm rstart

## iOS  

To run iOS, open up [`ios/SampleApp.xcodeproj`](ios/SampleApp.xcodeproj) in XCode, and click Run (the big play button).

## App Structure

    - android/ -----> native shell
    - ios/     -----> native shell
    - web/     -----> web shell
    - src/     -----> insert your code here!

### SRC folder

    - actions/      -----> Shared across  
    - constants/    -----> Shared across
    - dispatcher/   -----> Shared across
    - stores/       -----> Shared across
    - components/
    - index.android.js
    - index.ios.js
    - index.web.js

### Entry Points (index.*.js)

Following React-Native naming convention, we extend it by adding '.web' as indicator for web platform related files.

React-Native will be looking for index.js or index.ios.js or index.android.js

React + WebPack will take index.web.js as entry point for our Web js bundle.

### Components

    - components
      - MyComponent
        - MyComponent.native.js
        - MyComponent.web.js
        - MyComponentLogic.js
        - MyComponentRender.android.js
        - MyComponentRender.ios.js
        - MyComponentRender.native.js
        - MyComponentRender.web.js

#### Concepts:

 - Generic Component
 - Logic
 - Render

 Ideally every component has a main `Class` which inherits a base `Class` containing all the logic. Then, the main component import a different Render function which has been selected during the build. The file extension `.ios.js`, `.android.js` or `.js` is used by the build tool to import only the right file. The `.js` being picked for the Web portion.   

 React 0.14 announced the start point of the separation of react and react-dom. This would allow us to create that base react component that has no dependency on a DOM or a native View.

 Unfortunately as in 0.14.3 the dependency still exists.

 In order to mitigate this problem, we need to create a base component for native and one for web:
- MyComponent.native.js
- MyComponent.web.js.

Since we are forced to create these, we have selected `.native` and `.web` to avoid confusions on the targeted platform.

When the separation by React team is completed, we could have just one MyComponent.js thta creates a generic react component.    

#### Logic

MyComponentLogic is where the shared logic lives. Both native and web components will use it without any dependency on the DOM or native Views.

#### Render

    - MyComponentRender.android.js
    - MyComponentRender.ios.js
    - MyComponentRender.native.js
    - MyComponentRender.web.js

'.android' and '.ios' will be the ones react-native will look for. In the absence it will look for '.js'.

We have purposely avoided leaving '.js' files since it could be confusing if these are web or native related.

Instead, we enforce the use of '.web' as a convention.

What if you still want to have a common Render for ios and android. In that case, we delegate to a common file with '.native' extension.