## 🧠 What is useEffect?
useEffect is a React hook used to handle side effects in functional components — such as:

- Fetching data

- Subscribing/unsubscribing

- Setting up timers

- DOM manipulation

### ⚙️ Internal Working of useEffect in Steps
Let’s walk through it with the lifecycle of a functional component.


▶️ 1. Component Mounts
React renders your component for the first time.
```
useEffect(() => {
  console.log("Runs after initial render");
}, []);
```
Internally:

React stores your effect function in a queue linked to the component’s fiber node.

After the render is committed to the DOM, React runs the queued effects asynchronously (non-blocking).

⏱ useEffect does not run during the render phase — only after it completes.


🔁 2. Component Re-renders
Every time a re-render happens due to state/prop changes:
```
useEffect(() => {
  console.log("Runs when count changes");
}, [count]);
```
Internally:

React compares the current and previous dependency array ([count]).

If any dependency changed, React:

Runs the cleanup function from the previous effect (if any).

Then calls the new effect after the DOM update.


🔄 3. Cleanup Function
If you return a function from useEffect, it's used for cleanup:
```
useEffect(() => {
  const interval = setInterval(doSomething, 1000);
  return () => {
    clearInterval(interval); // cleanup
  };
}, []);
```
When Cleanup Runs:

Before the effect runs again (on update)

When the component unmounts


🧵 4. Effect Queueing & Ordering
React keeps a list of effects for each component.

Effects are run in order of declaration after the DOM is updated.

Cleanups happen before the next set of effects.


⚖️ 5. Synchronous vs Asynchronous
useEffect is async after render, not blocking paint.

React 18 may batch updates and flush effects together in idle time.

For synchronous behavior, useLayoutEffect is used (e.g., DOM measurements).

🔚 6. Component Unmounts
React runs all cleanup functions before removing the component from the DOM.
