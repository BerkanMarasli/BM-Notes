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
import { useState } from 'react'

const [reactive_value, setter] = useState(initial_state)
const [getValue, setValue] = useState(initial_state)

// Example
const [count, setCount] = useState(0)
```



## `UseEffect`

Replaces the component lifestyle methods from class based components.

![React lifecycle methods in Hooks](/Users/berkanmarasli/Desktop/BM-Notes/React_hooks.assets/Screenshot 2021-10-23 at 17.24.45.png)

```
```







