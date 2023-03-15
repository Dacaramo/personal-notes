# EAS

## General information ℹ️

**EAS** stands for **Expo Application Services**. A set of cloud based services made for **Expo** apps and **Bare React Native** apps.

> When developing with **Expo**, Tipically, EAS is used way after the initial **Expo** configuration, but it is always a good practice to set it up during the initial configuration and not only use Expo Go without generating any kind of build.

**EAS** is composed of 3 services: **EAS Build**, **EAS Submit** and **EAS Update**. 

## EAS Build 🔨

**EAS Build** allows you to build app binaries of your **Expo** and **Bare React Native** projects. It allows an easy way to build apps for distribution.

### Configure a project for EAS Build ⚙️

With an already created **Expo** project go to the root of the project and run `eas build:configure` and complete all the interactive console process by responding the questions asked by the cli, etc.

After running the previous command an `eas.json` file will be generated. The file will look like this (for a non bare project):

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal",
      "android": {},
      "ios": {}
    },
    "preview": {
      "distribution": "internal"
    },
    "production": {}
  }
}
```

The `eas.json` will contain your EAS Build configuration (and also EAS Submit configuration). Each of the props of the `"build"` object are **build profiles**. A **build profile** describes all the necessary parameters in order to generate a certain type of build. 

Inside each **build profile** object you can specify the props `"android"` and `"ios"` that will contain platform OS specific configuration props for the build. 

> For specifying build configuration props common to each platform just leave each prop at the root of your **build profile**. 

Check the **Expo** docs if you want to know more about all [the available props common to both platforms](https://docs.expo.dev/build-reference/eas-json/#options-common-to-both-platforms), [the available props only Android](https://docs.expo.dev/build-reference/eas-json/#android-specific-options) and [the available props only for iOS](https://docs.expo.dev/build-reference/eas-json/#ios-specific-options)

For sharing configuration props between **build profiles** you just need to use the `"extends"` prop with a value of the name of the build profile to inherit to.

### Run an EAS build profile and generate a build 👟

To generate a build with an specific profile run `eas build --profile <the name of your profile>`. **If you omit the `--profile` flag and the name of your profile then the command will generate a build for the `"production"` build profile (if it exists)**

## EAS Submit 🚀

**EAS Submit** allows you to upload and submit your app binaries to the app stores.

### Configure a project for EAS Submit ⚙️

The **EAS Submit** configuration goes also inside the `eas.json` file. If you don't now anything about the `eas.json` file go to the [[#Configure a project for EAS Build ⚙️]] section. The profiles that are going to be used by **EAS Submit** are called **submit profiles** instead of **build profiles**. The only difference is that the **submit profiles** are going to be under the `"submit"` object and not under the `"build"` object. Everything under the `"submit"` object is going to be used for the submit configuration.

> Everything said in the [[#Configure a project for EAS Build ⚙️]] section applies also for **EAS Submit**. The only different thing that you need to know is that [the available props common to both platforms](https://docs.expo.dev/build-reference/eas-json/#options-common-to-both-platforms), [the available props only Android](https://docs.expo.dev/build-reference/eas-json/#android-specific-options) and [the available props only for iOS](https://docs.expo.dev/build-reference/eas-json/#ios-specific-options) are obviously different.

## EAS Update 🔃

**EAS Update** allows you to generate updates for builds that were already released. 

## Environment variables with EAS 📔

**Environment variables** are able to be setted in 2 ways: 
1. By specifying the value of each variable with the command that runs a development server. (Works either with **Expo Go** or a **Dev Client**. This option will not work for production builds or builds where you are not able to run a development server locallly)
2. By generating a build that will set the environment variables at build time based on the build profile you choose.

> Please rename your `app.json` file to `app.config.js` and refactor it's content to look like JavaScript code.

Before you're able to set the value of each **environment variable** and use them inside your app codebase, for any of the 2 ways exposed above, you need to add the variables as props inside the `expo.extra` object inside the `app.config.js` file. Do it just like in the code sample below:

```JavaScript
export default {
  expo: {
  //...
    extra: {
      API_URL: process.env.API_URL || null,
    },
  },
};
```

### Way 1: Set the variables when running a development server

When starting your local development server for testing the app with **Expo Go** or a **Dev Client**, instead of just starting it with the command `expo start`, you need to prepend the variables and their value before: `API_URL="http://localhost:3000" expo start`

### Way 2: Specify the value of the variables inside `eas.json` in order to let the `eas-cli` set the variables when generating a build

Modify each **build profile** inside your `eas.json` file by adding an `"env"` prop. The `"env"` prop will be an object containing all of the **environment variables** that you want that the `eas-cli` sets for you. The `eas.json` file, compared to the want showed to you in the [[#Configure a project for EAS Build ⚙️]] section should look something like this:

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal",
      "env": {
        "API_URL": "https://localhost:3000"
      },
      "android": {},
      "ios": {}
    },
    "preview": {
      "distribution": "internal",
      "env": {
        "API_URL": "https://staging-api.com"
      }
    },
    "production": {
      "env": {
        "API_URL": "https://production-api.com"
      }
    }
  }
}
```

### Access and use the environment variables from your app codebase

Once you are sure that you will be able to access the value of the variables after setting them following either of the ways exposed above, you should:

1. Install the `expo-constants` module by typing `npx expo install expo-constants`
2. Import the module at the top of the file where you're going to use the variables and get the variables as it's showed below

```JavaScript
import Constants from 'expo-constants'
// ...
doSomething(Constants.expoConfig.extra.API_URL);
```

> Besides using the **environment variables** you have defined inside `app.config.js` you are able to use one of the [Built-in environment variables](https://docs.expo.dev/build-reference/variables/#built-in-environment-variables)

> If you want to rewrite the value of the **environment variables** that were already setted when the build was made just run the development server like in the [[#Way 1: Set the variables when running a development server]] section: `API_URL="http://localhost:8000" expo start`.

### Setting and getting environment variables when publishing an update

If you are going to publish an update and you want to also update some of the **environment variables** already setted when the build was generated just run: `API_URL="https://prod.example.com" eas update --branch production`
