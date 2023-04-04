# React Navigation

Before adding any kind of navigation to your app, the root of your app needs to be wrapped around a `<NavigationContainer/>`. This tag is going to wrap all of the other tags inside the entrypoint of your app, either `App.js` or `index.js`.

Install some key dependencies running the following 
- `npm install @react-navigation/native`
- `npm install @react-navigation/native-stack`
- `npm install @react-navigation/bottom-tabs`

Import and use the createNativeStackNavigator function:
```TypeScript
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();
```

## Move between screens
Every component that is a Screen receives by default a `navigation` object as a component prop. However, you will need to get the `navigation` object by calling the `useNavigation` hook if you need to trigger a navigation fron within other components that are not screens, just like this:
```TypeScript
const navigation = useNavigation();
```

See the [Navigation prop reference](https://reactnavigation.org/docs/navigation-prop/) for more details.

There are multiple navigators that allows multiple types of navigation, those are:
- Native Stack Navigator
- Tab Navigator
- Drawer Navigator

## Stack Navigator

**Purpose:** It allows a linear navigation between multiple screen components. Those screen components instances are saved inside a stack. All of the screens in the stack will preserve it's state and won't re run any effect when going back to them. **This navigator allows a navigation like the one provided by web browsers**.  

Import the function that creates and returns the navigator
```TypeScript
import { NavigationContainer } from '@react-navigation/native';
```

In order to navigate between screens just use `navigation.navigate(<screen-name>)` from your code.

See all of the [Native Stack Navigator details](https://reactnavigation.org/docs/native-stack-navigator).

## Tab Navigator

**Purpose:** Allows navigating between parallel screens from a bottom tab that holds all of the screens you are able to go to.

Import the function that creates and returns the navigator
```TypeScript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
```

In order to navigate between screens just use `navigation.navigate(<screen-name>)` from your code.

See all of the [Tab Navigator details](https://reactnavigation.org/docs/tab-based-navigation)

## Drawer Navigator

**Purpose:** Allows navigating between parallel screens from a left drawer that holds all of the screens you are able to go to.

Import the function that created ans returns the navigator
```TypeScript
import { createDrawerNavigator } from '@react-navigation/drawer';
```

To open, close and interact with the drawer you can call the following methods inside the `navigation` object:
    -   `jumpTo` - go to a specific screen in the drawer navigator
    -   `openDrawer` - open the drawer
    -   `closeDrawer` - close the drawer
    -   `toggleDrawer` - toggle the state, ie. switch from closed to open and vice versa

In order to navigate between screens just use `navigation.navigate(<screen-name>)` from your code.