---
title: 15 Essential React Interview Questions (With Answers)
description: A concise set of React interview questions with clear answers and code examples—covering state vs props, hooks, rendering, performance, and patterns.
date: "2025-08-24"
image: "/images/posts/post-1.jpg"
tags:
  - React
  - Interview
  - JavaScript
---

Preparing for a React interview? Here are concise, high-signal Q&As with code you can skim and practice.

## 1) Props vs State

- Props: external, read-only inputs to a component.
- State: internal, mutable via setState/useState.

```jsx
function Greeting({ name }) { // props
  const [count, setCount] = React.useState(0); // state
  return (
    <div>
      <p>Hello {name}</p>
      <button onClick={() => setCount(c => c + 1)}>{count}</button>
    </div>
  );
}
```

## 2) Why are keys needed in lists?

Keys help React identify which items changed, added, or removed for efficient reconciliation. Use stable, unique IDs—not indexes—when order can change.

```jsx
items.map(item => <Item key={item.id} {...item} />);
```

## 3) useEffect basics (deps and cleanup)

Run side effects after render; declare dependencies precisely. Cleanup runs before the next effect or on unmount.

```jsx
useEffect(() => {
  const id = setInterval(fetchData, 5000);
  return () => clearInterval(id); // cleanup
}, [fetchData]);
```

## 4) useMemo vs useCallback

- useMemo caches a computed value.
- useCallback caches a function reference.

```jsx
const total = useMemo(() => sum(list), [list]);
const onAdd = useCallback((x) => setList(l => [...l, x]), []);
```

## 5) Controlled vs Uncontrolled components

- Controlled: value driven by state; single source of truth.
- Uncontrolled: DOM holds state; access via refs.

```jsx
// Controlled
const [value, setValue] = useState("");
<input value={value} onChange={e => setValue(e.target.value)} />

// Uncontrolled
const ref = useRef();
<input ref={ref} defaultValue="hi" />
```

## 6) Preventing unnecessary re-renders

Use React.memo for pure components, useCallback/useMemo to stabilize props, and avoid creating new objects inline unless needed.

```jsx
const Child = React.memo(function Child({ onClick }) { /* ... */ });
```

## 7) Lifting state up

When two siblings need shared data, move state to their closest common ancestor and pass it down via props.

## 8) Context vs Redux

Context: lightweight prop drilling escape hatch. Redux/RTK: state management for complex apps (middleware, devtools, caching with RTK Query).

## 9) Error boundaries

Class components catching render/commit errors below them; use libraries (e.g., react-error-boundary) for hooks ergonomics.

```jsx
class Boundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError() { return { hasError: true }; }
  componentDidCatch(err, info) { /* log */ }
  render() { return this.state.hasError ? <Fallback/> : this.props.children; }
}
```

## 10) Lazy, Suspense, and code-splitting

Split bundles and suspense while loading.

```jsx
const Settings = React.lazy(() => import('./Settings'));
<React.Suspense fallback={<Spinner/>}>
  <Settings />
</React.Suspense>
```

## 11) useRef use cases

Persist values across renders without causing re-renders (DOM nodes, timers, instance vars).

```jsx
const idRef = useRef(null);
useEffect(() => { idRef.current = setInterval(tick, 1000); return () => clearInterval(idRef.current); }, []);
```

## 12) useReducer vs useState

Prefer useReducer for multi-field or complex transitions; co-locate logic and state updates.

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'inc': return { ...state, count: state.count + 1 };
    case 'reset': return { count: 0 };
    default: return state;
  }
}
const [state, dispatch] = useReducer(reducer, { count: 0 });
```

## 13) Derived state pitfalls

Don’t copy props to state unless you have a specific reason; derive on render or memoize.

## 14) React 18: automatic batching

Multiple state updates inside async callbacks batch by default, reducing extra renders.

```jsx
onClick={async () => {
  setA(1); setB(2); // batched
}}
```

## 15) Performance checklist

- Normalize data; avoid deep prop chains.
- Memoize expensive calculations and stable callbacks.
- Virtualize long lists (react-window, react-virtualized).
- Defer non-critical work (idle callbacks, Suspense streaming on server).
