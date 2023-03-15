# Routing

## The basics

**Next.js** is a SSR[^1] framework based on **React** that handles for you many configurations like routing. Configurations and functionalities that will take much more time to implement in **React** are able to be implemented easily with **Next.js**. Keep in mind that SSR will not be always the better choise. 

CSR[^2] stills to be the way to go if:
- You need more of a web app than a web page
- You need the user to log in and log out
- There's a lot of content of on the website that is private if you don't have an account

SSR is the way to go if:
- You need a website with good SEO[^3] in order to reach a lot of people via a search engine.

## Routing and Pages

Inside the `/app` directory:
- Each folder at the root level defines different routes for your app `
- Each folder translates as a URL segment (the URL segment will have the same name than the folder)
- Folders can be nested. That will result in a URL path with multiple nested URL segments, something like: `account/settings/language` where the `/language` folder is inside the `/settings` folder and the `/settings` folder is inside the `/account`.
- Leaf folders, that are not only for storing components or assets, will always contain a `page.js` file. Containing the `page.js` file makes that route available to the public and will allow the rendering of that page, otherwise trying to access that route will give you a **404 not found** error

If you want a new page just add a folder named as the URL segment you want and add a `page.js` file inside of it, the page will be mapped to that URL segment.

If you want that page 1, page 2 and page 3 will share a parent component (e.g. a navbar), then you must add at the root (at the `/app` folder level) a file named `layout.js`, then add 3 folders `/page1`, `/page2` and `/page3` with a `page.js` file inside each of them. The `layout.js` at the root will wrap those pages or components. The `layout.js` must specify the components that you want to share accross all pages, or the components that you always want to be the parent component of each of the pages. 

>Every component under the `/app` folder is a **Server Component**[^4] by default. If you want to define a component as a **Client Component** you need to add the string `'use client'` at the top of the file that has the component.

>Pages are **Server Components** by default but can be setted to **Client Components**.

>Pages can fetch data. View the [Data Fetching](https://beta.nextjs.org/docs/data-fetching/fundamentals) section for more information.

### Route groups

Naming a file to something like `/(something)` (always wrapped with parentheses) allows to prevent that folder to be mapped as a URL segment. So basically a folder structure that looks something like:
- This `/(company)/team/members` will translate to the following URL: `https://x.com/team/members`
- This `/company/(team)/members` will translate to the following URL: `https://x.com/company/members`

### Dynamic segments

If you don't now the name of the segment beforehand you can name the folder that will define the URL segment as something like `/[something]` (always wrapped with square brackets). So basically a folder structure that looks something like:
- This `/team/member/[memberId]` will translate to the following URL: `https://x.com/team/member/mem1256` if mem1256 was received by the page being rendered via it's params

## Layouts

A layout can be defined as a piece of UI that is shared between multiple pages. That shared piece of UI will preserve state, not re-render, and not re run effects when changing the child page inside of it, basically when navigating between pages, unless you navigate to a page that has another root layout.

Keep in mind that every layout you create must follow this rules in order to not throw an error:
- The file containing the layout must be always named as `layout.js`
- The component must receive a `children: ReactNode` prop as a component prop. that children prop will contain the page in which the user is located.
- The component must return the `children` wrapped by the html elements that you want in every page 

It is mandatory to have a root layout (a `layout.js` file at the level of the `/` folder). That root folder will need to always have a `<html/>` tag and a `<body/>` tag. If you specify a `<head/>` tag inside of it then the content of that tag will be the content specified inside the nearest `head.js` file inside the same folder of at the parent level. The root layout should look something like the following code:

```tsx
import { FC, ReactNode } from 'react';
import './styles/globals.css';

interface Props {
  children: ReactNode;
}

const RootLayout: FC<Props> = ({ children }) => {
  return (
    <html lang='en'>
      {/*
        <head /> will contain the components returned by the nearest parent
        head.tsx. Find out more at https://beta.nextjs.org/docs/api-reference/file-conventions/head
      */}
      <head />
      <body>{children}</body>
    </html>
  );
};
  
export default RootLayout;
```


>Layouts are **Server Components** by default but can be setted to **Client Components**.

>Layouts can fetch data. View the [Data Fetching](https://beta.nextjs.org/docs/data-fetching/fundamentals) section for more information.

>A `layout.js` and `page.js` file can be defined in the same folder. The layout will wrap the page

>The root layout is a [Server Component](https://beta.nextjs.org/docs/rendering/server-and-client-components) by default and **can not** be set to a [Client Component](https://beta.nextjs.org/docs/rendering/server-and-client-components#client-components)

## Templates

A template is the same as a layout but templates don't preserve state and will re-render when navigating to other child pages. When navigating to other child pages a new template instance will be created. it's not the same instance for all of the child pages.

Keep in mind that every template you create must follow this rules in order to no throw an error:
- The file containing the layout must be always named as `template.js`
- The component must receive a `children: ReactNode` prop as a component prop. that children prop will contain the page in which the user is located.
- The component must return the `children` wrapped by the html elements that you want in every page 

## Navigating between routes

There's 2 ways to navigate in Next.js: the `<Link/>` component and the `useRouter()` hook

### The `<Link/>` component

The `<Link/>` component allows you to define a clickable text link that will navigate the user to the path specified by the `href` prop passed to it. You can define a list of links to navigate to routes defined by dynamic segments, just like in the following example:

```jsx
import Link from 'next/link';

export default function PostList({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${post.slug}`}>
            {post.title}
          </Link>
        </li>
      ))}
    </ul>
  );
}
```

### The `useRouter()` hook

The `userRouter()` hook is used when you want to tie navigation to an html element like a button, just like in the following example:

```jsx
'use client';

import { useRouter } from 'next/navigation';

export default function Page() {
  const router = useRouter();

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  );
}
```

>Next.js recommends to use always navigation with the `<Link/>` component unless you need an specific functionality of this hook.

### Prefetching and preloading routes

Every route that is specified by the `href` prop of every `<Link/>` component being visible in the screen is going to be preloaded to improve performance. The preload will be made in the background. This preloading is not going to happen if you use `useRouter()` unless you use the `prefetch()` method of the hook.

## Handling loading states

You can show an instant loading state while the content of a route segment loads. Once rendering is complete the loading state will be swapped for the content of the route segment (could be a page or a layout). Loading indicators like loading skeletons and loading spinners are able to be displayed during the load state.

In order to add a loading state that will display when a page or a nested layout is being loaded, you need to add a `loading.js` file inside the folder that has one `layout.js` or a `page.js` file inside of it.

The `loading.js` file will create a **Suspense Boundary** that will show the loading UI until the childs of the boundary are rendered. The `loading.js` will create the boundary around every sibling component **including** the ones that are under nested folders, **except** `layout.js`.

If you have the following folder structure:

![Folder structure for loading.js example](./assets/fileStructureForLoadingExample.webp)

The boundary will be around the `page.js` inside `/dashboard`.

The created file should look like this:

```tsx
export default function Loading() {
  // You can add any UI inside Loading, including a Skeleton.
  return <LoadingSkeleton />
}
```

## Handling errors

In order to add an error UI and be able to handle runtime errors in a centralized way, add an `error.js` file inside a folder that contains an `app.js` or a `layout.js`.

The `error.js` will create an **Error Boundary** that will be ready to render an error component (the one inside `error.js`) when a exception or error is thrown within the boundary (e.g. when page.js throws an error). The `error.js` will create the boundary around every sibling component **including** the ones that are under nested folders, **except** `layout.js`, so if you have the following folder structure

![Folder structure for error.js example](./assets/fileStructureForLoadingExample.webp)

The boundary will be around the `page.js` inside `/dashboard`. Every error thrown by the `page.js` file  is going to trigger the render of the error component.

The created file should look like this: 

```tsx
'use client'; // Error components must be Client components

import { useEffect } from 'react';

export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error);
  }, [error]);

  return (
    <div>
      <h2>Something went wrong!</h2>
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </div>
  );
}
```

When the error component is active, layouts **above** the error boundary **maintain** their state and **remain** interactive, and the error component can display functionality to recover from the error. The `reset()` function will try to re-render all of the error boundary content, so in the example above only `page.js` will be re-rendered, if there where other folders down the hierarchy, then those layouts and pages are going to be re-rendered too.

If you want to include a `layout.js` inside the error boundary then you need to add another `error.js` file but at the parent level.

`error.js` at the root (inside the `/app` folder) is not going to put the root layout inside it's error boundary, if you want to put the root layout inside an error boundary you must use at that same level a file that is named `global-error.js`.

> Unlike the root `error.js`, the `global-error.js` error boundary wraps the **entire** application, and its error component replaces the root layout when active. Because of this, it is important to note that `global-error.js` **must** define its own `<html>` and `<body>` tags.

## Route handlers

The **Route Handlers**, also known as request handlers or API endpoints are basically functions that receive a request defined with an specific HTTP method, to an specific url, with or without url params and a request body if it applies.

It is difficult to understand why even those **Route Handlers** exists thinking in Next.js as a front-end framework, but Next.js works similar to old MVC frameworks that nowadays are used only as back-end framerorks like Spring, Django, Laravel, etc. So indeed Next.js is not directly comparable to React.js, Vue.js and Angular.js, those frameworks only works for front-end.

## Commands

Initialize a Next.js project: `npx create-next-app@latest`

[^1]: Server Side Rendering: When a web user access your web app, just the section of the app that is being visible, like the landing page, will be obtained from ther server, not all the project (e.g. Next.js project). A SSR framework will prerender the components on the server before the client receives it. So the server removes some of the work that the client's browser must do.
[^2]: Client Side Rendering: When a web user access your web app, all the project (e.g. React.js project) containing all of the javascript and html files will be fetched. 
[^3]: Serch Engine Optimization
[^4]: A component that during the app lifecycle will always be static. It will never change based on the user interaction. A **Server Component** could be a layout containing other components. The other components could be either **Server Components** or **Client Components**, it doesn't matter. **Next.js** suggest following the **Server First Philosophy** where every component that could be a **Server Component** should be a **Server Component**. This last thing will improve performance for your application.