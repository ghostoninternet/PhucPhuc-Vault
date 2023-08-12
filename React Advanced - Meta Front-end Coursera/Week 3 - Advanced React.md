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
**Specialization** defines components as being special cases of other c