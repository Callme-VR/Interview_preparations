# React Interview Master Guide: Theory & Practical Coding
**A Comprehensive Resource for Junior to Mid-Level React Developers**

---

## Table of Contents
1. [Core Fundamentals](#1-core-fundamentals)
2. [Hooks Deep Dive](#2-hooks-deep-dive)
3. [Lifecycle & Side Effects](#3-lifecycle--side-effects)
4. [Performance Optimization](#4-performance-optimization)
5. [State Management (Context, Redux, Zustand)](#5-state-management)
6. [API Integration & Data Fetching](#6-api-integration)
7. [React 18 & Modern Features](#7-react-18-features)
8. [Practical Coding Challenges](#8-practical-coding-challenges)
9. [Advanced Concepts & Testing](#9-advanced-concepts)

---

## 1. Core Fundamentals

### Q1: What is the Virtual DOM and how does React use it?
**Theory:** The Virtual DOM (VDOM) is a lightweight, in-memory representation of the real DOM. 
**Process:**
1. Whenever state changes, React creates a new VDOM tree.
2. It compares this new tree with the previous one (a process called **Diffing**).
3. React calculates the minimum number of changes needed.
4. It updates only those specific parts in the real DOM (**Reconciliation**).

### Q2: Difference between State and Props?
- **Props:** Data passed from parent to child (Immutable).
- **State:** Data managed within the component (Mutable).

### Q3: What is JSX?
**Theory:** Syntax extension for JS that looks like HTML. It's compiled to `React.createElement()` calls.

---

## 2. Hooks Deep Dive

### Q4: Explain the `useState` hook.
```javascript
const [count, setCount] = useState(0);
```

### Q5: What are the rules of Hooks?
1. Call at the top level only.
2. Call only from React functions (components or custom hooks).

### Q6: Difference between `useMemo` and `useCallback`?
- **`useMemo`:** Memoizes a **value** (result of a function).
- **`useCallback`:** Memoizes a **function instance**.

---

## 3. Lifecycle & Side Effects

### Q7: Explain `useEffect`.
- `[]`: componentDidMount.
- `[val]`: componentDidUpdate (specific).
- `return () => {}`: componentWillUnmount (cleanup).

---

## 4. Performance Optimization

### Q8: What is `React.memo()`?
HOC that prevents re-renders if props haven't changed.

### Q9: Code Splitting with `React.lazy`.
```javascript
const Component = lazy(() => import('./Component'));
```

---

## 5. State Management (Context, Redux, Zustand)

### Q10: Context API vs Redux vs Zustand?
| Tool | Best For | Complexity | Performance |
|---|---|---|---|
| **Context API** | Static/Low-frequency data (Themes, Auth). | Low | Can cause unnecessary re-renders in large trees. |
| **Redux (RTK)** | Large-scale apps, complex state transitions. | High | Excellent (via selectors and optimized middleware). |
| **Zustand** | Moderate to large apps, simple API, no boilerplate. | Low/Medium | High (atomic updates, no context providers). |

### Q11: What is Redux Toolkit (RTK) and why use it?
**Theory:** RTK is the official, recommended way to write Redux logic. It solves boilerplate issues (actions, constants, reducers) and includes built-in middleware like `redux-thunk` and Immer (for mutable-style state updates).
**Coding (Slice Example):**
```javascript
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; }, // Immer handles immutability
  },
});
export const { increment } = counterSlice.actions;
export default counterSlice.reducer;
```

### Q12: How does Zustand work?
**Theory:** Zustand is a small, fast state management library based on hooks. It doesn't require a Provider wrapper around your app.
**Coding:**
```javascript
import { create } from 'zustand';

const useStore = create((set) => ({
  count: 0,
  inc: () => set((state) => ({ count: state.count + 1 })),
}));

function Counter() {
  const { count, inc } = useStore();
  return <button onClick={inc}>{count}</button>;
}
```

---

## 6. API Integration & Data Fetching

### Q13: `fetch` vs `Axios`?
- **Fetch:** Native, requires two `.then()` calls (one for response, one for JSON), doesn't throw errors on 404/500 automatically.
- **Axios:** Third-party, automatic JSON transformation, built-in interceptors, better error handling (throws on non-2xx).

### Q14: What are Axios Interceptors?
**Theory:** Functions that Axios calls for every request/response. Great for adding Auth tokens or logging.
**Coding:**
```javascript
axios.interceptors.request.use(config => {
  config.headers.Authorization = `Bearer ${localStorage.getItem('token')}`;
  return config;
});
```

### Q15: Why use TanStack Query (React Query) instead of `useEffect`?
**Answer:** `useEffect` fetching lacks:
1. **Caching:** Storing data to avoid redundant API calls.
2. **Auto-refetching:** Updating data when window is refocused.
3. **Loading/Error States:** React Query provides `isLoading`, `isError` out of the box.
4. **Pagination/Infinite Scroll:** Simplified with built-in hooks.

---

## 7. React 18 & Modern Features

### Q16: What is Concurrent React?
**Theory:** React can now interrupt a rendering process to handle a more urgent update (like a user typing), making the UI feel more responsive.

### Q17: `useTransition` hook.
**Theory:** Allows you to mark updates as "transitions" (non-urgent), so they don't block the UI.
**Example:** Filtering a huge list while the user is still typing in the search box.

### Q18: What is Suspense for Data Fetching?
**Theory:** Allows components to "wait" for something (like an API call) before rendering, with a fallback UI (spinner).

---

## 8. Practical Coding Challenges

### Q19: Implement a "Debounced" Search.
**Theory:** Wait for the user to stop typing for `X` ms before calling the API.
```javascript
useEffect(() => {
  const timer = setTimeout(() => {
    fetchData(searchTerm);
  }, 500);
  return () => clearTimeout(timer);
}, [searchTerm]);
```

### Q20: Handle multiple API calls in parallel.
```javascript
const [res1, res2] = await Promise.all([
  fetch('/api/user'),
  fetch('/api/posts')
]);
```

---

## 9. Advanced Concepts & Testing

### Q21: What is Prop Drilling?
Passing data through components that don't need it. Fixed by Context/Redux/Zustand.

### Q22: What is an Error Boundary?
A class component that catches JS errors in its child tree.

### Q23: How to test React components?
- **Jest:** The test runner.
- **React Testing Library (RTL):** Focuses on testing how users interact with the UI rather than internal implementation details.
**Example:**
```javascript
test('renders welcome message', () => {
  render(<App />);
  const linkElement = screen.getByText(/welcome/i);
  expect(linkElement).toBeInTheDocument();
});
```

### Q24: Server-Side Rendering (SSR) vs Static Site Generation (SSG)?
- **SSR:** Page generated on the server for *every* request (Next.js `getServerSideProps`).
- **SSG:** Page generated *once* at build time (Next.js `getStaticProps`).

### Q25: What are React Server Components (RSC)?
**Theory:** Components that run only on the server. They reduce the amount of JS sent to the client and can access databases directly without an API layer.

---

## Summary for Quick Revision
- **State:** `useState` (Local), `useContext` (Global), `Redux/Zustand` (Large Global).
- **Effects:** `useEffect` (Side effects), `useLayoutEffect` (Synchronous DOM).
- **Performance:** `memo`, `useMemo`, `useCallback`, `lazy`, `Suspense`.
- **Modern:** Concurrent Mode, Transitions, Server Components.
