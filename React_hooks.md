# React Hooks

Only call hooks at the top level of a React functional componenet.



Basic Hooks:

- useState
- useEffect
- useContext

Additional Hooks:

- useReducer
- useCallback
- useMemo
- useRef
- useImperativeHandle
- useLayoutEffect
- useDebugValue



## `useState`

When the state changes, React updates UI to display changes.

```javascript
// useState
import { useState } from 'react'

const [reactive_value, setter] = useState(initial_state)
const [getValue, setValue] = useState(initial_state)

// Example
const [count, setCount] = useState(0)
```



## `UseEffect`

Combines all component lifestyle methods from class based components into one method.

![React lifecycle methods in Hooks](/Users/berkanmarasli/Desktop/BM-Notes/React_hooks.assets/Screenshot 2021-10-23 at 17.24.45.png)

```javascript
// useEffect without clean up
import { useEffect } from 'react'

useEffect(callback_function, [state_dependencies])

// Examples
// Run: 1) when mounted 2) when states change
useEffect(() => {
    alert('message')
})

// Run when mounted (when states are first initalised)
useEffect(() => {
    alert('message')
}, [])

// Run: 1) when mounted 2) when specified state dependencies change
useEffect(() => {
    alert('message')
}, [count_state])
```

```javascript
// useEffect with clean up
import { useEffect } from 'react'

useEffect(callback_function_with_cleanup, [state_dependencies])

// Example
// Run before component is removed from UI (unmounted)
useEffect(() => {
    alert('message')
    
    return () => {
        alert('goodbye message')
    }
})
```





















