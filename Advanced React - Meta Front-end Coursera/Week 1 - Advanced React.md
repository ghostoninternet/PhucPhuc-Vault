# Rendering Lists in React
**List**: Represents an array in JavaScript

One important advantage of using React is its ability to automatically optimize updates in your interfaces or UIs.

When computing a change, React applies it's stiffening algorithm to calculate the minimum number of changes that are necessary to perform an update in your tree of components.
![[Diffing Algo.png]]
### What are keys ?
> **Keys** are **identifiers' s** that help React to determine which item has changed, added or removed. They also instruct how to treat a specific element when an update occurs and whether its internal state should be preserved or not.
> The general rule of thumb with keys is to use a **stable identifier** that is unique among its siblings. This allows React to reuse as many elements from the list as possible, avoiding unnecessary recreations, especially their content is exactly the same and the only thing has changed is theri position in the list.

# Forms in React
> **Controlled components**: a set of components that offer a declarative application programming interface or API to enable full control of the state of form elements at any point in time using **React state**
> 
> **Value**: are special propert that React added to most of the form elements to determine the input content at any point in time during the render life cycle.

In order to create **controlled component**, you need to use a combination of **local state** and the **value prop** 

## Controlled components vs Uncontrolled components
> **Recap:**
> **Uncontrolled components** with refs are fine if your form is incredibly simple regarding UI feedback. However, **controlled input** fields are the way to go if you need more features in your forms.
> Evaluate your specific situation and pick the option that works best for you.

![[uncontrolled_controlled.png]]

| Reference |
| ----------- |
| https://formik.org/ |
| https://github.com/jquense/yup |
| https://github.com/react-hook-form/react-hook-form |

# React Context

