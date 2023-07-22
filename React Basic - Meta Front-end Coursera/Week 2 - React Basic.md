# Dynamic events and how to handle them
>The process by which the HTML button communicates to the JS event handler to execute some code and respond to the event action is known as triggering.
>
>In **React code**, events are handled using **JSX event attributes**, which are very similar to HTML event attributes. Example:
>			`<HTML>onclick` = `<JSX>onClick`
>Event groups:
>* Clipboard events
>* Composition events
>* Keyboard events
>* Mouse events
>* Selection events
>* Touch events
>* Wheel events
>* Animation events
>* ... and more !

## JavaScript
>1. "Plug into" HTML element on which to listen for an event.
>2. Use `addEventListener` method on the document object.
## React
> In React, the rule is to avoid manipulating the DOM directly as much as possible. You should set everything up declaratively. This is best done using event attribute, one-to-one mapping between HTML event attributes and JSX event attributes, but note that there is no function invocation syntax. Instead, you should pass a reference to the event handling function without invoking it.
> 
> One more feature only using React is the passing of function declarations as props.