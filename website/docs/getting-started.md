---
sidebar_position: 1
---

# Getting Started

## Pre-requisites

If you're already familiar with JavaScript, React Native and Webpack, then you'll be able to get moving quickly! Otherwise, it's highly recommended to get yourself familiar with these topic and then come back here:

- [React Native Express](https://www.reactnative.express/)
- [Webpack concepts](https://webpack.js.org/concepts/)

## Minimum requirements

- `react-native` >= 0.62.0
- Node >= 14 (__recommended Node 16 or newer__)

If you're using older versions of React Native, you can still try using Re.Pack, but your mileage may vary as they are not officially supported.

## Note on Expo

Officially, Re.Pack doesn't support Expo. It might be possible to use Re.Pack in Expo projects, but it would require more work and setup. We welcome anyone who would like to contribute and bring Expo support to Re.Pack.

Additionally, in order to use [Code Splitting](./code-splitting/concept) functionality, Re.Pack provides a native module which should be compiled into your application. This means that with Expo you won't be able to use it.

## Compatibility with Webpack

On paper, __Re.Pack should work with any version of Webpack 5__, but we recommend to consult with the compatibility table below.
The table represents versions of `webpack` for which Re.Pack is confirmed to work correctly.

If you don't see your version, give it a go. If it doesn't work, please open an issue.


| `webpack`  | `@callstack/repack`                     |
| ---------- | --------------------------------------- |
| `5.22.0`   | `1.0.x`, `1.1.x`, `1.2.x`               |
| `>=5.29.0` | `1.2.x`, `1.3.x`, `1.4.x`, `2.x`, `3.x` |

## Examples

If you get stuck or you want to explore beforehand, you can use the examples of React Native applications using Re.Pack here: https://github.com/callstack/repack-examples.

## Installation

### 1. Dependencies

Install required dependencies in your project:

```bash
npm i -D webpack terser-webpack-plugin babel-loader @callstack/repack
# or
yarn add -D webpack terser-webpack-plugin babel-loader @callstack/repack
```

This will install latest versions of Webpack, Re.Pack and dependencies used in Webpack config: `terser-webpack-plugin` for minification and `babel-loader` for transpiling the code.

If you already have Webpack or Re.Pack installed, you might want to check the [compatibility table](#compatibility-with-webpack) to ensure all dependencies are ok.

Once the dependencies are installed, you need to tell React Native CLI to add Re.Pack's commands. 

### 2. React Native config

Add the following content to `react-native.config.js` (or create it if it doesn't exist):

```js
module.exports = {
  commands: require('@callstack/repack/commands')
};
```

This will allow you to use `react-native webpack-start` and `react-native webpack-bundle` commands, but before that you need to create a Webpack configuration.

### 3. Webpack config

Create file `webpack.config.mjs` in the root directory of your project and paste the content from our [Webpack config template](https://github.com/callstack/repack/blob/main/templates/webpack.config.mjs).

:::info

We recommend to use ESM version of Webpack config with the `.mjs` extension. However, Re.Pack also supports ESM under `.js` and CJS variant under `.js` and `.cjs` extensions. Check our [templates](https://github.com/callstack/repack/blob/main/templates/) for CJS and ESM variants as well as the documentation on [Webpack config](./configuration/webpack-config) to see the list of all available Webpack config location and extensions.

:::

### 4. Configure XCode

When building release version of your application XCode project will still use Metro to bundle the application, so you need to adjust build settings to make XCode use Re.Pack instead.

Open your application's XCode project/workspace and:

1. Click on the project in **_Project navigator_** panel on the left
2. Go to **_Build Phases_** tab
3. Expand **_Bundle React Native code and images_** phase
4. Add `export BUNDLE_COMMAND=webpack-bundle` to the phase

After the change, the content of this phase should look similar to:

```bash
export NODE_BINARY=node
export BUNDLE_COMMAND=webpack-bundle
../node_modules/react-native/scripts/react-native-xcode.sh
```

### 5. Configure Android

When building release version of your application Gradle will still use Metro to bundle the application, so you need to adjust build settings to make Gradle use Re.Pack instead.

Open your application's Gradle project, usually located at `android/app/build.gradle` and add `bundleCommand: "webpack-bundle"` to `project.ext.react`.

After the change, the content of `project.ext.react` should look similar to:

```groovy
project.ext.react = [
    enableHermes: false,  // clean and rebuild if changing
    bundleCommand: "webpack-bundle",
]
```

## Usage

After completing [Installation](#installation) you should be able to use Re.Pack's development server and bundle your application.

Keep in mind that, you might need to adjust [Webpack config](./configuration/webpack-config) to your project in order to the application to work correctly. It's impossible for us to know what your project looks like and uses, so it's recommended to read through the Webpack config's code and comments and make sure you understand what's going on there.

### Running development server

When developing your application, you want to run Re.Pack's development server to compile your source code with Webpack.

**The recommended way is to use React Native CLI and run:**

```bash
react-native webpack-start
```

:::tip

You can pass the same CLI options to `react-native webpack-start` as you would to `react-native start`.

:::

### Bundling the app

When building the release version of your application via XCode, Gradle CLI or Android Studio, as long as you followed [Configure XCode](#4-configure-xcode) and [Configure Android](#5-configure-android), it should use Re.Pack to bundle your application.

If you want to create bundle (development or production), **the recommended way is to use React Native CLI and run**:

```bash
react-native webpack-bundle
```
:::tip

You can pass the same CLI options to `react-native webpack-bundle` as you would to `react-native bundle`.

:::

The 2nd option, is to use Webpack CLI:

```bash
webpack -c webpack.config.mjs --env platform=ios --env mode=production
```

Make sure you have `webpack-cli` installed in your project.

:::caution

Using Webpack CLI is recommended **only for advanced users**, who have previous experience with Webpack. 

:::
