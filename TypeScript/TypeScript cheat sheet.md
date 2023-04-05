# TypeScript cheat sheet

## When a library don't have types

> Before creating the types for any library check if there's indeed no types for it, go to the Definitely typed repo and look for them.

> If you find them you just need to run `npm install @types/<module-name>`

You must create your own `/@types` folder at the root of your project. Inside that folder create a folder for each library that don't have types and for which you want to create them.

As an example we are going to create types for the lodash library, so inside `/@types` create a folder named `/lodash`. Inside of it create a file named `index.d.ts`. The content of `index.d.ts` must look like the following:

```TypeScript
// Inside the module bellow add an export for each item you want to define it's types
declare module 'lodash' {
	export function random(lowerLimit: number, upperLimit: number): number
}
```

