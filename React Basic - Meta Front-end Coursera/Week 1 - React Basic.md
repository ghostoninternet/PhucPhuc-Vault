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

