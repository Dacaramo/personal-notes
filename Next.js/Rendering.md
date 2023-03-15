# Rendering

When using Next.js there are 2 types of components: **Server Components** and **Client Components**.

## Client Components

A component that is going to be rendered on the client browser that pulls the React project. Client components are rendered when the user is already navigating the website, so those components will take more time to show. **Components must be Client Components when they have a dynamic behaviour: As a general rule if a component has state attached to it and uses hooks like `useState()`, `useEffect()`, `createContext()` or `useContext()`, etc. it should be defined as a Client Component. Also a component must be a Client Component when you're assigning event handlers to event props as `onChange()`, `onClick()`, etc. **

> To define a **Client Component** you will need to add the string `'use client'` at the top of the file containg the component that you want to define as a **Client Component**.

> In order to share data between **Client Components** you will need to use the built in **React Contexts** or a state management third party library.

> In Next.js 13, **React Contexts** is fully supported within **Client Components**, but they **cannot** be created or consumed directly within **Server Components**. You must create the context inside another file that defines a **Client Component**, not at the root layout (the root layout is a **Server Component**) if you do that last thing you will get an error. Only after creating the context as the suggested way you will be able to insert the context provider at the root of the root layout.

To improve the performance of your application, we recommend moving **Client Components** to the leaves of your component tree where possible.

For example, you may have a Layout that has static elements (e.g. logo, links, etc) and an interactive search bar that uses state.

Instead of making the whole layout a **Client Component**, move the interactive logic to a **Client Component** (e.g. `<SearchBar />`) and keep your layout as a **Server Component**. This means you don't have to send all the component Javascript of the layout to the client.

> There's some third party libraries (components) that doesn't have the `'use-client'` directive yet. If those components don't have the directive and you use them directly inside a **Server Component** you will get an error. It is a good practice, at least if the official documentation of the library does not even tell if it has the directive, to wrap those third party components with a component build by you that will return the third party component directly with the only difference that the file will have the directive at the top.

> Although it's possible to fetch data in **Client Components**, Next.js recommends fetching data only in **Server Components** unless you have a specific reason for fetching data on the client. Moving data fetching to the server leads to better performance and user experience.

## Server Components

A component that is going to be rendered on the web server that holds the React project. Server components are rendered before they're delivered to the client, so they will show almost instantly when the user navigates to the page that contains them. Next.js recommends using always a Server First approach, where every component that could be a **Server Component** will be indeed a **Server Component**. No need to have additional slow **Client Components**. Just add **Client Components** when it is strictly necessary. 

> By default every component under the `/app` folder is a **Server Component**, so you will never have to define a component as a **Server Component**, because that's the default.

> In order to share data between **Server Components** you can't use contexts, instead you are able to use JavaScript design patterns like global singletons. (e.g. a module that will export a database connection that is going to be established one single time inside of that module, but the connection is able to be used in any of the **Server Components** components)

Both **Server Components** and **Client Components** can optimize it's rendering in 2 ways: **Static Rendering** and **Dynamic Rendering**.

### Static Rendering

With **Static Rendering** the components are pre-rendered at **build time** on the server. After being pre-rendered the components will be cached so the pre-render will not need to be done each time the components are requested. That pre-render will reduce the time that the server will take to return the components to the client when they're requested.

**Static Rendering** is the default type of rendering for both **Client Components** and **Server Components**

>If you need to, you're allowed to pre-render again the components, that process is called **revalidation**.

### Dynamic Rendering

With **Dynamic Rendering**, both Server and Client Components are rendered on the server at **request time**. The result of the work is not cached, so the components are rendered each time their requested.

### How to insert a Server Component inside a Client Component?

In order to insert a **Server Component** inside a **Client Component** you can't just import the component and place it inside the open and closing tag of a **Client Component**, that is going to give you an error.

Instead you have 2 options:
- Make a wrapper component that is a **Client Component** and wrap the **Server Component** with it.

Create the wrapper component:

```tsx
'use client';

export default function ClientComponent({children}) {
  return (
    <>
      {children}
    </>
  );
}
```

And wrap the Server Component with it:

```tsx
import ClientComponent from "./ClientComponent";
import ServerComponent from "./ServerComponent";

// Pages are Server Components by default
export default function Page() {
  return (
    <ClientComponent>
      <ServerComponent />
    </ClientComponent>
  );
}
```

- The other option consist on passing the **Server Component** as a prop of a **Client Component**, so the **Client Component** is able to add the **Server Component** tag tag on it's return.

>You can't pass props that are not serializable from a **Server Component** to a **Client Component**.

### Keeping Server-Only code out of the Client Components

Sometimes there's a node module created and exported by you that it's intended to be used only on the server. A common case for this is if you have a module that uses a key that is only present on the server. If that's the case then the code will never work if it is used on the client. Keep in mind that you are able to prevent that from happening.

To prevent this sort of unintended client usage of server code, we can use the `server-only` package to give other developers a build-time error if they ever accidentally import one of these modules into a **Client Component**. Install the module by running `npm install server-only`

Import the package on every **Client Component** that is likely to use a module that is only intended to be used by the server, just like in the following example:

```js
import "server-only";

export async function getData() {
  let resp = await fetch("https://external-service.com/data", {
    headers: {
      authorization: process.env.API_KEY,
    },
  });

  return resp.json();
}
```

After doing that, any **Client Component** that imports the `getData()` function will receive immediately a build-time error.

> There's also a package called `client-only` if you need to prevent the opposite. e.g. when you have a module that access the window object and you could accidentally use it on the server.