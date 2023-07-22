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

# Data and Events
>The advantages of utilizing a centralized point of data - a **"single source of truth"**:
>1. It offers a more efficient way of working when data frequently changes.
>2. It reduces the possibility of typing errors in your code.
>3. It allows you to edit multiple items from a single point.![[parent_child_data_flow_React.png]]
>
>In React, data flow is a one-way street. Sometimes it's said that the data flow is unidirectional. Put differently, the data in React flows from a parent component to a child component. The data flow starts at the root and can flow to multiple levels of nesting, from the root component (parent component) to the child component, then the grandchild component, and further down the hierarchy.
>
>Props are immutable (cannot be changed).
>
>The two main benefits of this unidirectional data flow are that it allows developers to:
>1. comprehend the logic of React apps more quickly and 
>2. simplify the data flow.
>
>Having data move through props in only one direction makes it simpler to understand the logic of how the components interact. If data were moving everywhere, all the time, then it would be much harder to comprehend its logical flow. Any optimization you tried to implement would likely not be as efficient as it could be, especially in modern React.
### Why is one-way flow in React is important ?
>1. Ensures data is moving from top to bottom through the component hierarchy.
>2. Changes are transmitted through the system.

>All data in React can be divided into **props data** and **states data**:
>**props data**: Data outside the components that it receives and works with but can't mutate.
>**state data**: Data inside the components that it controls and can mutate.

## Hooks
>One key benefit of hooks is that they solve the problem of unnecessary code reuse across components.
>
>**HOOKS**: functions that let you hook into **React state** and lifecycle features from components.
>**useState hook**: this hook is used to manage the state within a component and keep track of it.
>	1. Import the **useState** `import React, {useState} from 'react'`
>	2. Declare a **state variable** `const [state, setState] = useState(initialState)`
>	3. Provide any name `const [showMenu, setShowMenu] = useState(false)` 
>Calling **useState** will do two things:
>1. Create a state variable with an initial value --> `showMenu`
>2. Creates a function to set that state variable's value --> `setShowMenu` 
>**useState** hook should be called at the top level of your component. You can use this hook to track any kind of data: string, number, arrays, Booleans or objects.
>
>Hooks also come with a set of rules, that you need to follow while using them:
>1. You can only call hooks at the top level of your component or your own hooks.
>2. You cannot call hooks inside loops or conditions.
>3. You can only call hooks from React functions, and not regular JavaScript functions.

## State
>State is data in a component that determines behavior.

