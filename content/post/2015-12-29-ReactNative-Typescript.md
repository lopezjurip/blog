+++
categories = ["mobile", "react-native", "typescript"]
date = "2015-12-30T09:01:44-03:00"
description = "How to write a React Native app using Typescript"
keywords = ["mobile", "react-native", "typescript"]
title = "React Native + Typescript"
+++

[React-Native](https://facebook.github.io/react-native) is a big deal, you should try it.

By default, it comes with [Flow](http://flowtype.org) support, but in this case we will setup a project using [Typescript](http://www.typescriptlang.org). It's awesome how Typescript increases in popularity thanks to projects like [Angular2](https://angular.io) and how all the community maintains the typings.

> See the complete project and more complex code on: [**github.com/mrpatiwi/ReactNativeTS**](https://github.com/mrpatiwi/ReactNativeTS)

## Requirements

Follow the official requirements on [facebook.github.io/react-native/docs/getting-started.html#requirements](https://facebook.github.io/react-native/docs/getting-started.html#requirements).

### Setup

Install [Homebrew](http://brew.sh):

```sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Install [nvm](https://github.com/creationix/nvm) (Node Version Manager):

```sh
brew update && brew install nvm
mkdir ~/.nvm
```

Add this to your `~/.bash_profile` or `~/.zshconfig`:

```sh
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
```

Install the lastest version of Node:

```sh
nvm install node && nvm alias default node
```

Install Facebook's `watchman`:

```sh
brew install watchman
```

Install `react-native-cli`, Typescript and [DefinitelyTyped/tsd](https://github.com/DefinitelyTyped/tsd):

```sh
npm install -g react-native-cli typescript tsd
```

## Init Project

Let's create a project named `ReactNativeTS`:

```sh
react-native init ReactNativeTS
```

We will remove `.flowconfig`:

```sh
rm .flowconfig
```

## Init Typescript settings

This will create:

*   `tsconfig.json`: Settings to tell Typescript how to *compile* our code.
*   `tsd.json`: Typings version manager

```sh
tsd init && tsc --init
```

We will modify `tsconfig.json` and set it as:

```json
{
    "compilerOptions": {
        "target": "es6",
        "jsx": "react",
        "noImplicitAny": true,
        "experimentalDecorators": true,
        "preserveConstEnums": true,
        "outDir": "built",
        "rootDir": "src",
        "sourceMap": true
    },
    "filesGlob": [
        "typings/**/*.d.ts",
        "src/**/*.ts",
        "src/**/*.tsx"
    ],
    "exclude": [
        "node_modules"
    ],
    "compileOnSave": false
}
```

> *   We will compile to ES6 (ES2015).
> *   Use React support
> *   The input code have to be placed at `src`.
> *   The output code will be placed at `built`
> *   `compileOnSave` is for your IDE, in my case Atom.

**Make sure to add `built` to your `.gitignore`**:

```gitignore
# .gitignore

# Typescript Output
built
```

### Dependencies

Let's install some dependencies:

```sh
npm install --save-dev typescript gulp gulp-typescript concurrently
```

*   [`gulp`](http://gulpjs.com) as our building system.
*   [`concurrently`](https://www.npmjs.com/package/concurrently) so we can watch and have started the ReactNative in the same terminal session.

## Gulpfile

Now, we have to setup the `gulpfile.js` to declare our build pipeline:

```js
var gulp = require('gulp');
var ts = require('gulp-typescript');

// Grab settings from tsconfig.json
var tsProject = ts.createProject('tsconfig.json');

gulp.task('build', function() {
    var tsResult = tsProject.src().pipe(ts(tsProject));
    return tsResult.js.pipe(gulp.dest('built'));
});

gulp.task('watch', ['build'], function() {
  gulp.watch('src/**/*.ts', ['build']);
  gulp.watch('src/**/*.tsx', ['build']);
});

gulp.task('default', ['build']);
```

Now we have the following avilable on the command line:

*   `gulp build`: Take all the Typescript code and produces the `.js` code.
*   `gulp watch`: Every change on a `.ts` or `.tsx` file will trigger `gulp build`.

## Typings

Let's install the typings we need:

```sh
tsd install --save react-native
```

This will update the `tsd.json` like this:

```json

{
  "version": "v4",
  "repo": "borisyankov/DefinitelyTyped",
  "ref": "master",
  "path": "typings",
  "bundle": "typings/tsd.d.ts",
  "installed": {
    "react-native/react-native.d.ts": {
      "commit": "dc9dabe74a5be62613b17a3605309783a12ff28a"
    },
    "react/react.d.ts": {
      "commit": "dc9dabe74a5be62613b17a3605309783a12ff28a"
    }
  }
}
```

And the `typings` directory as:

```
ReactNativeTS
└── typings
    ├── react-native
    ├── react
    └── tsd.d.ts
```

## Hello world

Let's modify the entry points of the app:

```js
// index.ios.js

'use strict'

import { AppRegistry } from 'react-native'
import App from './built'

AppRegistry.registerComponent('ReactNativeTS', () => App)
```

```js
// index.android.js

'use strict'

import { AppRegistry } from 'react-native'
import App from './built'

AppRegistry.registerComponent('ReactNativeTS', () => App)
```

Yes, they are identical. React-Native automatically picks the `.android` or `.ios` file from a local dependency, in this case the `index.js` of `build`.

### Typescript files

Let's convert the default hello world page to `.tsx`, we will start with the iOS part:

Import dependencies:

```tsx
// src/index.ios.tsx

/// <reference path="../typings/tsd.d.ts"/>

import React from "react-native";
const { StyleSheet, Text, View } = React;
```

The `style` remains the same:

```tsx
const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
        backgroundColor: "#F5FCFF",
    },
    welcome: {
        fontSize: 20,
        textAlign: "center",
        margin: 10,
    },
    instructions: {
        textAlign: "center",
        color: "#333333",
        marginBottom: 5,
    },
});
```

And the `App` class will be like this:

```tsx
// We are exporting it
export default class App extends React.Component<any, any> {
    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.welcome}>
                    Welcome to React Native
                </Text>
                <Text style={styles.instructions}>
                    To get started, edit index.ios.js
                </Text>
                <Text style={styles.instructions}>
                    Press Cmd+R to reload, {"\n"}
                    Cmd+D or shake for dev menu
                </Text>
            </View>
        );
    }
}
```

The `src/index.android.tsx` is very similar:

```tsx
// src/index.android.tsx

/// <reference path="../typings/tsd.d.ts"/>

import React from "react-native";
const { StyleSheet, Text, View } = React;


const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: "center",
        alignItems: "center",
        backgroundColor: "#F5FCFF",
    },
    welcome: {
        fontSize: 20,
        textAlign: "center",
        margin: 10,
    },
    instructions: {
        textAlign: "center",
        color: "#333333",
        marginBottom: 5,
    },
});


export default class App extends React.Component<any, any> {
    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.welcome}>
                    Welcome to React Native!
                </Text>
                <Text style={styles.instructions}>
                    To get started, edit index.android.js
                </Text>
                <Text style={styles.instructions}>
                    Shake or press menu button for dev menu
                </Text>
            </View>
        );
    }
}
```

## Run the app

I recommend adding this scripts to `package.json`:

```json
"scripts": {
    "build": "gulp build",
    "watch": "gulp watch",
    "start": "concurrent \"npm run watch\" \"node node_modules/react-native/local-cli/cli.js start\" ",
    "android": "adb reverse tcp:8081 tcp:8081 && react-native run-android"
}
```

Run it!:

```sh
npm start
```

{{% figure src="/images/reactnativets-terminal.png"%}}

## See the result

### iOS

Open `ios/ReactNativeTS.xcodeproj` with XCode and press play.

{{% figure src="/images/reactnativets-ios.png"%}}

### Android

See [facebook.github.io/react-native/docs/android-setup.html](https://facebook.github.io/react-native/docs/android-setup.html).

{{% figure src="/images/reactnativets-android.png"%}}

# Conclusions

We can see that it's 100% possible to write React-Native apps with Typescript.

The problem comes with maintainability, because the React-Native team is moving fast and the typings will not have official support.

Maybe it's better to stick with Flow.
