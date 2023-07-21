# Course Introduction
>**State**: The values of all variables your app is working with at any given point.

>React is a declarative, efficient, and flexible JavaScript library for building UIs. It lets you compose complex UIs from small and isolated pieces of code called "components". React apps are built using modern JavaScript features, which are commonly known as ES6. Developers use React to develop **Single Page Application (SPA)**.

>**Single Page Application (SPA)**: This is a one-page website where some of the pages content changes based on user interaction.
>**Component-based architecture**: Build software based on reusable components of code. **Reusable**: used multiple times and easily inserted anywhere where needed --> **Independent**: Components can exist within the same space independently from each other. 
>**Components**: Stand alone parts of a UI. Components are **self-contained**, they have their own HTML, CSS, and JavaScript logic for functionality.

# React Components and Where They Live

>React provides two types of components: **Functional components** and **Class components**.
>Every React app must contain at least **one** component, and it's called the **root component**.

**Render syntax**: `<ComponentName/>`
![[React_pic1.png]]

>**React** uses a special syntax called **JavaScript XML** or **JSX**. JSX syntax looks very similar to **HTML**. Think JSX as a combination of **custom HTML** and **JavaScript**. You can place this syntax inside the **return function** of a functional component. **React component** must be used as a JSX element.

>The **first letter** of the name of a component file must be **Capitalize**. This is because there's a difference in how **React** treats capitalized and non capitalized component names. **React** treat lowercase component as **regular HTML elements**. Capitalize component names help **React** to distinguish **JSX element** from **HTML element**.
>The first letter of the name of the function inside **App.js** must also be capitalized. 

>**Transpiling**: Interpreting a programming language and translating it to a specific target language. Using this, **React** can convert JSX to HTML.
>**A browser cannot understand JSX syntax**
>--> Need **transpiler**: this technology takes a piece of code and transforms it into some other code.

>**Folder Structure of a React App**
>
>**The src folder**: contains all the essential component files required to ensure that a React app functions.
>**App.js and index.js**: used to render root components of the app.
>**App.css**: contains the styles for the **App.js** components. **index.css** contains the style to use in the entire app.
>**App.test.js, setupTests.js and reportWebVitals.js**: files related to the app's performance and testing.
>The **most important** file in **src folder** is **index.js**. This file imports everything that this **React app** needs to render a working **React app**.
>
>**The root folder**:
>**package.json**: lists information pertaining to my app, which allows **npm** to run several scripts and perform various tasks in the app itself. 
>**package-loc.json**: holds the list of all dependencies with a specific versions. The **package-loc.json** file help **npm** rebuild the app on another machine

>**Modules**: Stand-alone units of code that you can re-use again and again.
>**export**: make a module available in another module.
>There are two types of export in JS:
>1. The **default export**: `export default App;`
>Used when the function name is the same as the file name
>2. The **named export**: `export {App};`
>Used when you want the function name to be different from the file name.
>**Component**: small piece of functionality. **Module**: a series of components. 
>**Modular programming**: splitting your code into several modules. This complements the **component-based architecture** of React.
>**import**: `import <name of component you want> from <the file location containing the component>`

# Component Use and Styling

### React Props
>1. Pass data between components
>2. Arguments are passed like HTML attributes
>3. Uses the keyword props
>4. Send multiple data types
>5. Flexible dynamic content
>When two components communicate with each other, the component **sending** the **props** data is known as **the parent** and the component receiving the data is known as **the child**. It's also possible for parent components to send the same data to multiple child components. This communication is **one-directional** data flow.
>
>**Pure functions**: functions that always returns the **same output** for the same arguments values. In React, when you declare a component using props, it **MUST NEVER MODIFY ITS OWN PROMPTS**. 

### JSX
>Allow developers to write HTML directly inside the JavaScript code.
>**NOTE**: A regular JS function is used to define how **React** should render the component wherever it's referenced using the JSX syntax.

### Props and children
>`props.children`: a special prop that is automatically passed to every component.