### **1. What is React and how does it work?**
**Explanation:**
React is a JavaScript library for building user interfaces. It is based on the concept of components, which represent a part of the UI. React uses a virtual DOM to improve performance by updating only the changed parts of the real DOM.

Key Features:
- **Declarative**: You describe what you want, and React updates the UI.
- **Component-Based**: Reusable, modular UI components.
- **Unidirectional Data Flow**: Ensures predictable state updates.

---

### **2. What are React hooks, and why were they introduced?**
**Explanation:**
Hooks allow functional components to use state and lifecycle features, which were previously available only in class components. Introduced in **React 16.8**, they simplify code and reduce boilerplate.

Key Hooks:
- `useState`: Manage state.
- `useEffect`: Handle side effects like API calls.
- `useContext`: Access global state.

Why hooks?
1. Avoid complex class hierarchies.
2. Improve code reuse with custom hooks.
3. Enable easier testing and debugging.

---

### **3. What is JSX, and why is it used in React?**
**Explanation:**
JSX stands for JavaScript XML. It's a syntax extension that allows you to write HTML within JavaScript.

Example:
```jsx
const element = <h1>Hello, world!</h1>;
```

Why JSX?
- Improves code readability.
- Allows embedding logic within UI structure.
- Compiles to `React.createElement` during runtime.

---

### **4. Explain the differences between controlled and uncontrolled components.**
**Explanation:**
- **Controlled Components**: React controls the form inputs via state. Example:
  ```jsx
  const [value, setValue] = useState('');
  <input value={value} onChange={e => setValue(e.target.value)} />;
  ```

- **Uncontrolled Components**: DOM manages the state via refs. Example:
  ```jsx
  const inputRef = useRef();
  <input ref={inputRef} />;
  ```

Controlled components are recommended for predictable and testable behavior.

---

### **5. What is the difference between `useEffect` and `componentDidMount`?**
**Explanation:**
- `componentDidMount`: Used in class components for side effects after the component renders.
- `useEffect`: The modern hook equivalent for functional components. It can mimic:
  - **`componentDidMount`**: Use an empty dependency array (`[]`).
  - **`componentDidUpdate`**: By specifying dependencies.
  - **`componentWillUnmount`**: Return a cleanup function.

Example:
```jsx
useEffect(() => {
  console.log("Mounted");
  return () => console.log("Cleanup");
}, []);
```

---

### **6. What are higher-order components (HOCs) in React?**
**Explanation:**
HOCs are functions that take a component and return a new component. They enhance the behavior of a wrapped component.

Example:
```jsx
function withLogger(WrappedComponent) {
  return function EnhancedComponent(props) {
    console.log("Rendered");
    return <WrappedComponent {...props} />;
  };
}
```

They are used for cross-cutting concerns like logging, authentication, etc. Note: HOCs are being gradually replaced by hooks.

---

### **7. How do React's context and `useContext` work?**
**Explanation:**
Context provides a way to pass data through the component tree without prop-drilling.

1. **Create Context**: `const MyContext = React.createContext();`
2. **Provide Data**:
   ```jsx
   <MyContext.Provider value={data}>
     <Child />
   </MyContext.Provider>
   ```

3. **Consume Data** (with `useContext`):
   ```jsx
   const value = useContext(MyContext);
   ```

Introduced in **React 16.3**, Context works well for global states.

---

### **8. What is React's reconciliation process?**
**Explanation:**
Reconciliation is the process by which React updates the DOM. The Virtual DOM compares the current and previous states of the UI to find changes (diffing algorithm).

Steps:
1. Create a virtual DOM tree.
2. Compare virtual DOM trees to detect differences.
3. Update only the affected nodes in the real DOM.

This optimization reduces performance overhead.

---

### **9. What are lazy loading and `React.lazy`?**
**Explanation:**
Lazy loading delays the loading of components until they're needed, improving initial load times. `React.lazy` allows components to be rendered on-demand.

Example:
```jsx
const LazyComponent = React.lazy(() => import('./MyComponent'));
<Suspense fallback={<div>Loading...</div>}>
  <LazyComponent />
</Suspense>
```

Supported from **React 16.6**, it pairs with `Suspense` for error handling and fallback UIs.

---

### **10. What is React Fiber, and how does it improve React?**
**Explanation:**
React Fiber is a reimplementation of React's core algorithm (introduced in **React 16**). It improves rendering performance by splitting tasks into smaller units.

Key Features:
- **Concurrent Rendering**: Prioritizes tasks to keep the UI responsive.
- **Time Slicing**: Pauses and resumes rendering tasks as needed.
- Better error handling via new lifecycle methods.

---

### **11. How does React's `useReducer` hook differ from `useState`, and when should you use it?**
**Explanation:**
`useReducer` is used for complex state logic, while `useState` is ideal for simple state management.

- **`useState` Example**:
  ```jsx
  const [count, setCount] = useState(0);
  ```

- **`useReducer` Example**:
  ```jsx
  const reducer = (state, action) => {
    switch (action.type) {
      case 'increment':
        return { count: state.count + 1 };
      case 'decrement':
        return { count: state.count - 1 };
      default:
        throw new Error();
    }
  };
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  ```

`useReducer` is recommended for scenarios like form handling or complex state transitions.

---

### **12. Explain the differences between class components and functional components.**
**Explanation:**
Class components are ES6 classes that extend `React.Component`, while functional components are simpler and defined as JavaScript functions.

- **Class Components**:
  - Have lifecycle methods (e.g., `componentDidMount`, `shouldComponentUpdate`).
  - Use constructors and `this`.

- **Functional Components**:
  - Introduced in **React 16.8** with hooks.
  - Use `useEffect`, `useState`, etc., to manage side effects and state.

Functional components are preferred due to simpler syntax and better performance.

---

### **13. What are the best practices for optimizing React applications?**
**Explanation:**
Key techniques for optimization:
1. **Code Splitting**: Use `React.lazy` and `Suspense` to load components on demand.
2. **Memoization**:
   - Use `React.memo` to prevent unnecessary re-rendering of functional components.
   - Use `useCallback` for memoizing event handlers.
3. **Avoid Inline Functions**: They create a new reference on every render.
4. **Virtualization**: Optimize lists with libraries like `react-window`.

---

### **14. What is the significance of keys in React?**
**Explanation:**
Keys help React identify which items in the list have changed, are added, or removed. This improves performance during reconciliation.

Example:
```jsx
const listItems = items.map(item => <li key={item.id}>{item.name}</li>);
```

Using unique keys ensures React's diffing algorithm works efficiently.

---

### **15. Can you explain the difference between `Context` and `Redux`?**
**Explanation:**
- **Context**: Native React feature for lightweight state management across components.
  - Suitable for small, app-specific data (like theme or locale).
  
- **Redux**: External library for managing complex state across large-scale applications.
  - Suitable for global application data (like authentication, user data).

When combined, Context can manage local state while Redux handles global application state.

---

### **16. What are React Portals, and how do they work?**
**Explanation:**
Portals allow rendering a React component outside its parent DOM hierarchy.

Example:
```jsx
ReactDOM.createPortal(<Child />, document.getElementById('portal-root'));
```

Portals are useful for modals, tooltips, and popups. Introduced in **React 16**.

---

### **17. How does the `useRef` hook differ from `useState`?**
**Explanation:**
`useRef` maintains a mutable reference to an element or value without triggering re-renders, whereas `useState` updates and re-renders the component.

Use Cases:
- Storing DOM references:
  ```jsx
  const inputRef = useRef();
  <input ref={inputRef} />;
  ```
- Persisting values between renders without triggering updates.

---

### **18. What are React Fragments, and why are they used?**
**Explanation:**
Fragments enable grouping of children without adding extra DOM nodes.

Example:
```jsx
<>
  <h1>Hello</h1>
  <p>World</p>
</>
```

Advantages:
- Clean, lightweight DOM.
- Useful for grouping children returned from components.

---

### **19. What is the role of `React.StrictMode`?**
**Explanation:**
`React.StrictMode` helps identify potential problems in an application during development.

Benefits:
1. Highlights deprecated methods.
2. Detects unsafe lifecycle methods.
3. Double-invokes functions like `useEffect` to identify side-effects.

It does not affect production builds.

---

### **20. Explain the difference between `useEffect` and `useLayoutEffect`.**
**Explanation:**
- **`useEffect`**: Executes after rendering. Suitable for API calls, subscriptions.
- **`useLayoutEffect`**: Executes synchronously after DOM mutations but before painting. Used for measuring DOM elements or fixing layout issues.

Example:
```jsx
useEffect(() => console.log("Effect"));
useLayoutEffect(() => console.log("LayoutEffect"));
```

---

### **21. What is `useMemo`, and how does it improve performance?**
**Explanation:**
`useMemo` memoizes the output of a computation to prevent unnecessary recalculations during re-renders.

Example:
```jsx
const computedValue = useMemo(() => {
  return expensiveFunction(data);
}, [data]);
```

When to use:
- For expensive calculations (e.g., filtering large arrays).
- To optimize performance in components with heavy computations.

---

### **22. How does the `useCallback` hook differ from `useMemo`?**
**Explanation:**
`useCallback` memoizes a function, whereas `useMemo` memoizes the output of a computation.

Example:
```jsx
const memoizedCallback = useCallback(() => {
  performAction();
}, [dependencies]);
```

Use cases:
- Prevents re-creation of callback functions passed to child components.
- Improves performance by reducing unnecessary renders.

---

### **23. Explain React's `Suspense` and its role in data fetching.**
**Explanation:**
`Suspense` allows rendering of fallback UI while waiting for asynchronous operations (e.g., data fetching).

Example:
```jsx
<Suspense fallback={<Loading />}>
  <Component />
</Suspense>
```

With libraries like React Query, `Suspense` can simplify handling loading states.

---

### **24. What are the new features introduced in React 18?**
**Explanation:**
Key features in React 18 include:
1. **Concurrent Rendering**: Improved responsiveness.
2. **`useTransition`**: Enables seamless UI transitions.
3. **Automatic Batching**: Groups multiple state updates into a single render.
4. **Server Components**: Allows rendering components server-side.

---

### **25. How does React's `useTransition` enhance user experience?**
**Explanation:**
Introduced in **React 18**, `useTransition` delays UI transitions for non-urgent updates.

Example:
```jsx
const [isPending, startTransition] = useTransition();
startTransition(() => {
  setState(newState);
});
```

Benefits:
- Keeps the UI responsive.
- Prevents blocking on heavy computations.

---

### **26. What is hydration, and how does React handle it?**
**Explanation:**
Hydration occurs when React binds event listeners to HTML rendered by the server. It's essential for server-side rendering (SSR).

Steps:
1. Render static HTML on the server.
2. Send HTML to the client.
3. Use React's `hydrate` method to attach event listeners.

Example:
```jsx
ReactDOM.hydrate(<App />, document.getElementById('root'));
```

---

### **27. How do React's Error Boundaries work?**
**Explanation:**
Error Boundaries catch JavaScript errors in their child components and display fallback UI without crashing the app. Created using class components.

Example:
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  componentDidCatch(error, info) {
    logError(error, info);
  }
  render() {
    return this.state.hasError ? <h1>Error Occurred</h1> : this.props.children;
  }
}
```

---

### **28. What is `React.memo`, and how is it different from `useMemo`?**
**Explanation:**
`React.memo` is a higher-order component that memoizes functional components, preventing re-renders when props remain unchanged.

Example:
```jsx
const MemoizedComponent = React.memo(Component);
```

Difference:
- `useMemo` memoizes values/computations.
- `React.memo` memoizes whole components.

---

### **29. How does the `useImperativeHandle` hook work, and when should you use it?**
**Explanation:**
`useImperativeHandle` customizes the instance value that a parent accesses via `ref`.

Example:
```jsx
useImperativeHandle(ref, () => ({
  focus: () => inputRef.current.focus(),
}));
```

Use case:
- Exposes imperative methods for complex interactions between parent and child components.

---

### **30. What is Tree Shaking, and how does it benefit React apps?**
**Explanation:**
Tree Shaking removes unused code during the build process, reducing bundle size.

How it works:
- Uses ES6 module imports (`import/export`).
- Tools like Webpack analyze dependency graphs to eliminate dead code.

Benefits:
- Improves load times.
- Optimizes React applications for production.

---

### **31. What is the significance of Concurrent Mode in React?**
**Explanation:**
Concurrent Mode, introduced in **React 18**, allows React to interrupt rendering tasks to handle high-priority updates, improving UI responsiveness. 

Key Features:
- **Time Slicing**: Splits rendering work into small chunks.
- **useTransition**: Helps manage UI states during transitions.

Benefits:
- Enhances performance for complex UI interactions.
- Keeps apps responsive even during heavy computations.

---

### **32. How does the `useDeferredValue` hook work?**
**Explanation:**
Introduced in **React 18**, `useDeferredValue` delays the update of a value to improve UI performance in situations where immediate updates might cause lag.

Example:
```jsx
const deferredValue = useDeferredValue(value);
```

Use cases:
- Useful for filtering large lists or typing in search inputs while keeping the UI smooth.

---

### **33. What are React Server Components?**
**Explanation:**
React Server Components (RSC), introduced experimentally in **React 18**, enable rendering components on the server. 

Key Features:
- Reduces bundle size by excluding server-rendered components from the client.
- Improves performance by fetching data server-side.

Example structure:
```jsx
// .server.js component
export default function MyServerComponent() {
  return <div>Rendered on the server</div>;
}
```

---

### **34. What are render props, and how are they used?**
**Explanation:**
Render props is a pattern where a function is passed as a prop to dictate what to render.

Example:
```jsx
function DataProvider({ render }) {
  const data = useFetchData();
  return render(data);
}

<DataProvider render={(data) => <DisplayData data={data} />} />;
```

Benefits:
- Flexible composition.
- Shares logic between components without HOCs.

---

### **35. How does React handle reconciliation in lists with dynamic data?**
**Explanation:**
React uses keys to identify elements and handle efficient updates in lists.

Challenges:
1. Adding unique identifiers for dynamic data.
2. Avoiding the use of indices as keys (leads to bugs in UI updates).

Example:
```jsx
const items = data.map((item) => <li key={item.id}>{item.name}</li>);
```

---

### **36. What is the significance of `React.StrictMode` for development?**
**Explanation:**
`React.StrictMode` runs additional checks to highlight potential problems in the code.

Features:
- Warns about unsafe lifecycle methods.
- Double-invokes certain functions like `useEffect` to ensure idempotence.
- Alerts on deprecated APIs.

It is a development-only feature and has no impact on production builds.

---

### **37. What are custom hooks, and how do they improve code reusability?**
**Explanation:**
Custom hooks allow extracting reusable logic from components, enabling better abstraction and reusability.

Example:
```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  useEffect(() => {
    fetch(url).then((res) => res.json()).then(setData);
  }, [url]);
  return data;
}
const data = useFetch('https://api.example.com/data');
```

Benefits:
- Reduces code duplication.
- Simplifies complex components.

---

### **38. Explain the difference between `useEffect` cleanup and `componentWillUnmount`.**
**Explanation:**
- **`useEffect` cleanup**: Used in functional components to clean up effects like subscriptions, timers, or listeners.
- **`componentWillUnmount`**: Used in class components for similar purposes.

Example:
```jsx
useEffect(() => {
  const timer = setInterval(() => console.log('Tick'), 1000);
  return () => clearInterval(timer); // Cleanup
}, []);
```

Functional components with `useEffect` offer finer control over dependencies.

---

### **39. What is code-splitting, and how does React support it?**
**Explanation:**
Code-splitting breaks the app into smaller bundles to improve performance.

React supports code-splitting via:
1. **Dynamic Imports**: Load components asynchronously.
   ```jsx
   const Component = React.lazy(() => import('./Component'));
   ```
2. **`React.Suspense`**: Displays fallback UI while loading.

Tools like Webpack and Rollup further enhance code-splitting.

---

### **40. What are synthetic events in React?**
**Explanation:**
Synthetic events are React's wrapper around browser events, providing consistent behavior across different browsers.

Example:
```jsx
function handleClick(event) {
  console.log(event.type); // "click"
}
<button onClick={handleClick}>Click Me</button>;
```

Benefits:
- Normalized event handling.
- Improved performance with React's event delegation system.


---

### **41. How does React's Virtual DOM differ from the Real DOM?**
**Explanation:**
The Virtual DOM is a lightweight JavaScript representation of the real DOM. React uses it to minimize direct manipulations of the DOM.

Key Differences:
1. **Performance**: The Virtual DOM allows React to batch and optimize changes before updating the Real DOM.
2. **Efficiency**: Changes are compared using a diffing algorithm, reducing unnecessary updates.

---

### **42. What is the difference between React's `createElement` and JSX?**
**Explanation:**
- `React.createElement`: Creates React elements manually.
  ```jsx
  React.createElement('div', { className: 'container' }, 'Hello');
  ```

- JSX: A syntactic sugar that compiles to `React.createElement`.
  ```jsx
  <div className="container">Hello</div>;
  ```

JSX improves code readability but isn't required to use React.

---

### **43. How does `useEffect` handle dependencies, and what issues can arise?**
**Explanation:**
`useEffect` runs based on the dependency array. Missing or incorrect dependencies may cause:
1. **Unnecessary re-renders**.
2. **Stale closures**: Accessing old values if dependencies aren't updated.

Example:
```jsx
useEffect(() => {
  fetchData(id); // `id` should be in dependencies
}, [id]);
```

---

### **44. What are `React.forwardRef` and its use cases?**
**Explanation:**
`React.forwardRef` passes refs from parent to child components.

Example:
```jsx
const Input = React.forwardRef((props, ref) => (
  <input {...props} ref={ref} />
));
```

Use cases:
- Accessing DOM nodes in deeply nested components.
- Interacting with third-party libraries.

---

### **45. How do hooks enable better composition compared to HOCs and render props?**
**Explanation:**
Hooks integrate directly into functional components, avoiding the complexity and nesting caused by HOCs or render props.

Advantages:
1. Simplifies sharing logic.
2. Reduces boilerplate.
3. Improves readability.

---

### **46. How do you handle performance bottlenecks in a React app?**
**Explanation:**
Steps to optimize performance:
1. **Memoization**: Use `useMemo`, `React.memo`, and `useCallback`.
2. **Code Splitting**: Use dynamic imports with `React.lazy`.
3. **Virtualization**: Optimize long lists with libraries like `react-window`.

---

### **47. What are the benefits and drawbacks of Server-Side Rendering (SSR) in React?**
**Explanation:**
**Benefits**:
- Faster initial load (pre-rendered HTML).
- SEO improvements.

**Drawbacks**:
- Increased server load.
- Complex setup.

---

### **48. What are React's lifecycle phases, and how do hooks fit in?**
**Explanation:**
Lifecycle phases:
1. **Mounting**: `componentDidMount`, `useEffect([])`
2. **Updating**: `componentDidUpdate`, `useEffect([dependencies])`
3. **Unmounting**: `componentWillUnmount`, `useEffect cleanup`

Hooks simplify lifecycle management by combining these phases in a single `useEffect`.

---

### **49. What is the significance of fragments in React, and how do they differ from divs?**
**Explanation:**
Fragments (`<>...</>`) group elements without adding extra nodes to the DOM.

Example:
```jsx
<>
  <h1>Hello</h1>
  <p>World</p>
</>
```

Divs create unnecessary DOM nodes, affecting performance and styles.

---

### **50. How do React's Suspense and Error Boundaries complement each other?**
**Explanation:**
- **Suspense** handles loading states for asynchronous operations.
- **Error Boundaries** catch runtime errors in child components.

Together, they improve resilience and user experience.

---
