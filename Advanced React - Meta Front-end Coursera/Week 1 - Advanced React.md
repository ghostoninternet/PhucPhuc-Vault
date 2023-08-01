# Rendering Lists in React
**List**: Represents an array in JavaScript

One important advantage of using React is its ability to automatically optimize updates in your interfaces or UIs.

When computing a change, React applies it's stiffening algorithm to calculate the minimum number of changes that are necessary to perform an update in your tree of components.
![[Diffing Algo.png]]
### What are keys ?
> **Keys** are **identifiers' s** that help React to determine which item has changed, added or removed. They also instruct how to treat a specific element when an update occurs and whether its internal state should be preserved or not.
> The general rule of thumb with keys is to use a **stable identifier** that is unique among its siblings. This allows React to reuse as many elements from the list as possible, avoiding unnecessary recreations, especially their content is exactly the same and the only thing has changed is theri position in the list.