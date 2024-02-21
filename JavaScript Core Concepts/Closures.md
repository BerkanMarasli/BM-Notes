<h1> Closures </h1>

Resources:

- ColorCode's YouTube video on closures: https://www.youtube.com/watch?v=aHrvi2zTlaU&t=916s

<h2> Description </h2>

A closure is the combination of a function bundled together with references to its surroundings - MDN web docs.

In other words, a closure gives you access to an outer function's scope from an inner function.

Closures **remember** the outer function scope even **after** creation time.

<h2> Examples </h2>

```javascript
// Example 1: inner function has scope of outer function

function outerFunc(n) {
  function innerFunc() {
    console.log(`inner function n: ${n}`)
  }
}

outerFunc(2) // inner function n: 2
```

```javascript
// Example 2: example of outer function scope being remembered even after creation

function outerFunc(n) {
  function innerFunc() {
    console.log(`inner function n: ${n}`)
  }
  
  return {
    innerFunc
  }
}

const refToOuterFunc = outerFunc(2)
refToOuterFunc.innerFunc() // inner function n:2
```

Because the inner function still has a reference to the outer function's environment, that environment is considered reachable and is not garbage-collected. This allows the inner function to acess variables from the outer function even after it has finished executing.

<h2> Practical Example </h2>

```javascript
// Practical example: using click handlers

// Multiple buttons on the page, selected, and fontsize applied but DRY broken
document.getElementById('button-12').onclick = function() { document.body.style.fontsize = '12px' }
document.getElementById('button-14').onclick = function() { document.body.style.fontsize = '14px' }
document.getElementById('button-16').onclick = function() { document.body.style.fontsize = '16px' }

// Step 1: Extract to a function (wont work)
function clickHandler(size) {
  document.body.style.fontsize = `${size}px`
}
document.getElementById('button-12').onclick = clickHandler(12) // will not work as onclick wants the reference to a function whereas clickHandler(12) here is invoking the function (we need to pass in the size somehow)

// Therefore, we need clickHandler to return a function
function clickHandler(size) {
  return function() { document.body.style.fontsize = `${size}px` }
}
document.getElementById('button-12').onclick = clickHandler(12) // now works, possible via closure
```

