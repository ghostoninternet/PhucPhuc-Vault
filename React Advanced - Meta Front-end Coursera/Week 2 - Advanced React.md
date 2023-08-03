# Getting Started with Hooks

# Rules of Hooks and Fetching Data with Hooks

There are 4 rules of hooks:
1. Only call hooks from a **React component** function 
	You should only call hooks from inside a **React component** function, from a **built-in hook call**, from a **custom hook**.
2. Only call hooks at the top level of a **React component** function
	You must call your hooks before a return statement, outside of loops, condition or nested function
3. Call multiple states or effect hooks inside a **React component** function
	There can be multiple hook calls as long as they are **in the same order**. What this mean is that you can't place hook calls **inside conditional** because that might result in an invocation of a hook being skipped when compared with the previous render.
4. Make multiple hook calls in the same sequence
	Don't disrupt the sequence of invocation from one render to the next. If you want to call an effect conditionally you can still do so, but make sure to place the condition inside the hook
---
> **Fetch**: used to make server requests to retrieve some **JSON** data
> **Single-thread execution**: it means the next step can't start unless the previous step has finished.

Fetching data from a third-party API is considered **a side-effect** when working with React. Being a side-effect, you need to use the **useEffect hook** to deal with using the Fetch API calls in React.