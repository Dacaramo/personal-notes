# FlatList

`<FlatList/>` is a **React Native** component that works as an alternative for the traditional `Array.prototype.map` method, where you need to create a `<ListItem/>` component and return one of them per each item that is inside the array that contains the request results.

Learn the [FlatList details](https://reactnative.dev/docs/flatlist)

# Pagination with infinite scroll

When fetching some data, in order to display it inside your app, you will need to send a request to one of your API endpoints, or send a request to a public API endpoint. In any case, the endpoint that handles the request must offer some kind of pagination solution like query cursors.

In React, or in other framework, pagination is accomplished by getting not all the results in one request, but getting the results in smaller batches. when the user scrolls down, or when he specifies that he wants to get more results then you get them.

To display some spinner or loading indicator when the end of the list is reached then you must pass some component to the `ListFooterComponent` prop of the `<FlatList/>` item.

Besides that, you will need to call the function that sends the request again when the end of the list is reached. Fot that you need to set the prop `onEndReach` to a function that sends the request passing the query cursor or the id of the last readed item, so the endpoint is capable of returning the correct results.

Finally, you need to set the `onEndReachedThreshold` prop to a number between 0 and 1 that tells how far from the end the bottom edge of the list must be from the end of the content to trigger the function setted to the `onEndReach` function.

