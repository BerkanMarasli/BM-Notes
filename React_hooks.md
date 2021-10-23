# React Hooks

Only call hooks at the top level of a React functional componenet.



Basic Hooks:

- useState
- useEffect
- useContext

Additional Hooks:

- useRef
- useReducer
- useMemo
- useCallback
- useImperativeHandle
- useLayoutEffect
- useDebugValue

Utility Hooks:

- useToggle
- useTimeout
- useDebounce
- useUpdateEffect
- useArray



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



## `useContext`

useContext uses React's context API which is used to share data between components wihtout passing props.

All the components that need access to the context informaion needs to be wrapped around `<ContextName.Provider value={Contect_value}></ContextName.Provider>`.

```javascript
// App.js (parent component)
import React, { useState } from 'react'
import FunctionComponent from './FunctionComponent'

export const ContextName = React.createContext()

export default function App(){
    const [darkTheme, setDarkTheme] = useState(true)
    return (
        <ContextName.Provider value={darkTheme}>
    		<FunctionComponent />
    	</ContextName.Provider>
    )
}

// FunctionComponent.js (child component)
import React, { useContext } from 'react'
import { ContextName } from '.App'

export default function FunctionComponent() {
    const darkTheme = useContext(ContextName)
    return (
        <h1>Dark theme is {darkTheme}</h1>
    )
}
```



## `useRef`

Used to grab native HTML elements from JSX (grab elements from the DOM).

useRef does not re-render UI.

```javascript
// useRef
import { useRef } from 'react'

function App() {
    const myBtn = useRef(null)
    
    const clickIt = () => myBtn.current.click()
    
    return (
        <button ref={myBtn}></button>
	)
}
```



## `useReducer`



## `useMemo`

Linked to performance?



## `useCallback`

Linked to performance?



## `useImperativeHandle`



## `useLayoutEffect`



## `UseDebugValue`



https://www.youtube.com/watch?v=0c6znExIqRw&ab_channel=WebDevSimplified





























