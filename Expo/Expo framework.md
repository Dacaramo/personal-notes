# Expo framerork

## General information ℹ️

**Expo** is a React Native based framework to develop cross platform apps. The advantages of using **Expo** over **React Native** is that **Expo** will manage each OS configuration of your app, that means that you're never likely to touch an `/android` folder or an `/ios` folder inside your codebase.

## Configuration that every Expo project must have ⚙️

> Every enterprise level Expo project must have the configuration exposed in this section. It is a good practice to start doing this with every Expo project you do, that will ensure that you learn all the best practices.

- Convert the `app.json` to an `app.config.js` file
- Install the `expo-constans` library

## Every `expo-cli` and `eas-cli` Command ⌨️

- **Account related commands:**
	- Install the expo-cli globally: `npm install -g expo-cli`
	- Install expo-constants library for using env. variables: `npx expo install expo-constants`
	- Create an Expo project: `npx create-expo-app <the name of your app>` 
	- Login to your expo account: `eas login`
	- Check which user is logged in: `eas whoami`
	- Logout from your expo account: `eas logout`
- EAS Build commands:
	- Configure your project for EAS Build: `eas build:configure`