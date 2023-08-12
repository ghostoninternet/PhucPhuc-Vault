# JSX Deep Dive
**JSX**: syntax extension to JavaScript that **React** uses to **describe** what the UI should look like.

When React analyzes your rendering methods from all your components, it takes the whole **JSX tree** starting from **the root** and creates an **intermediary representation**. This representation is essentially another tree similar to before, but where each node instead of being JSX is a **plain object** describing a **component instance** or **DOM node** and its desired properties. This plain object is what React defines as **an element**.
![[Pasted image 20230812203306.png]]
**An element** is just a way to represent **the final HTML output** as a plain object. It consists primarily of two attributes: **type** and **props**. **Type** describes the **type of node** and **props** encompassess all the properties the component receives in a single object.
When React creates the entire element tree starting from the root, the root element specifies all the **child elements** as the **children prop**. And each **child element** does the same thing until it reaches the end of the tree. What important about this structure is that both child and parent elements are just **descriptions** and not **actual instances**.
The type of an element can also be **a function** corresponding to a **React component**. This is the **fundamental idea of React**, both **user defined components** and **DOM nodes** can be nested and mixed with each other in the elementary. Once React finishes the process of identifying all **user defined components** from the **tree of elements**, it will **convert** them into **DOM elements**. The result is what is generally known as **the virtual DOM** - a JavaScript alternative representation of the real DOM.
![[Pasted image 20230812205257.png]]


### What are the steps involved when there is a new change in your UI ?
First, React takes all your JSX and produce a new UI representation as a tree of elements.
Second, it will compares with the previous representation that is kept in memory.
Third, it will calculate the difference in a tree, recall that since each node in the tree is a JavaScript object, this diffing operation is very fast.
Finally, based on that difference, it will apply the minimum number of changes to the underlying DOM nodes in order to process the update

---
There are **2 main features** that enable **component composition**: **containment** and **specialization**. 
**Containment** refers to the fact that some components don't know **their children** ahead of time.
**Specialization** defines components as being special cases of other components

## Types of Children
>In JSX expressions, the content between an opening and closing tag is passed as a unique prop called children. There are several ways to pass children, such as rendering **string literals** or using **JSX elements** and **JavaScript expressions**. It is also essential to understand the types of JavaScript values that are ignored as children and don’t render anything.
>
>>**STRING LITERALS**
>>String literals refer to simple JavaScript strings. They can be put between the opening and closing tags, and the children prop will be that string.
>>`<MyComponent>Little Lemon</MyComponent>`
>>In the above example, the `children prop` in `MyComponent` will be simply the string `“Little Lemon”`.
>>
>>There are also some rules JSX follows regarding whitespaces and blank lines you need to bear in mind, so that you understand what to expect on your screen when those edge cases occur.
>>1. JSX removes whitespaces at the beginning and end of a line, as well as blank lines
>>2. New lines adjacent to tags are removed
>>3. JSX condenses new lines that happen in the middle of string literals into a single space
>
>>**JSX Elements**
>>You can provide JSX elements as children to display nested components. JSX also enables mixing and matching different types of children, like a combination of string literals and JSX elements.
>>
>>A React component can also return a bunch of elements without wrapping them in an extra tag. For that, you can use **React Fragments** either using the explicit component imported from React or **empty tags**, which is a shorter syntax for a fragment. A React Fragment component lets you group a list of children without adding extra nodes to the DOM. You can learn more about fragments in the additional resources unit from this lesson.
```JavaScript
return (
  <React.Fragment>
    <li>Pizza margarita</li>
    <li>Pizza diavola</li>
  </React.Fragment>
);

return (
  <>
    <li>Pizza margarita</li>
    <li>Pizza diavola</li>
  </>
);
```
>>**JavaScript Expressions**
>>You can pass any JavaScript expression as children by enclosing it within curly braces `{}`.
>
>>**Functions**
>>Suppose you insert a JavaScript expression inside JSX. In that case, React will evaluate it to a string, a React element, or a combination of the two. However, the children prop works just like any other prop, meaning it can be used to pass any type of data, like functions.
>
>>**Booleans, Null and Undefined are ignored**
>>false, null, undefined, and true are all valid children. They simply don’t render anything.



