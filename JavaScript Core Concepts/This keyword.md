<h1> This Keyword </h1>

Resources:

- ColorCode's YouTube video on this keyword: https://www.youtube.com/watch?v=fVXp7ZWjlO4&t=1207s

<h2> Description - normal functions</h2>

`this` is a way of referencing 'something' that is calling a function.

`this` is whoever is invoking the function.

```javascript
function talk() {
  return this
}

const me = {
  name: 'me'
  talk
}

me.talk() // returns { name: 'me', talk: func } as me object calls the talk function
```

Another example:

```javascript
function talk() {
  return `Hi, ${this.name}`
}

const me = {
  name: 'me'
  talk
}

const you = {
  name: 'you'
  talk
}

me.talk() // Hi, me
you.talk() // Hi, you
```

<h3> Binding using Function.bind </h3>

You can use `.bind` to configure the value of `this` for a function. For example:

```javascript
function talk() {
  return `Hi, ${this.name}`
}

const me = {
  name: 'me'
}

const newTalkFunc = talk.bind(me) // here we bind the value of this to the object me
newTalkFunc() // Hi, me
```

<h3> Function.call </h3>

Binding returns a new function. If you dont want to return a new function, rather just call it, then use `.call`. For example:

```javascript
function talk() {
  return `Hi, ${this.name}`
}

const me = {
  name: 'me'
}

talk.call(me) // Hi, me
```

You can pass in additional arguments to the function. For example:

```javascript
function talk(surname) {
  return `Hi, ${this.name} ${surname}`
}

const me = {
  name: 'me'
}

talk.call(me, 'Polo') // Hi, me Polo
```

<h3> Alternative to Function.call is Function.apply </h3>

Difference being you pass in the additional parameters as an array to argument 2. For example:

```javascript
function talk(surname) {
  return `Hi, ${this.name} ${surname}`
}

const me = {
  name: 'me'
}

talk.apply(me, ['Polo']) // Hi, me Polo
```







<h2> Description - constructor functions</h2>

With constrcutor functions, the binding happens automatically. The `new` keyword does some things in the background:

```javascript
function Person(n) {
  this = {} // background
  this.name = n
  return this // background
}

// Therefore code will look like this:
class Person(n) {
  this.name = n
}

const polo = new Person('Polo') // Person { name: 'Polo' }
```

Another example:

```javascript
function Person(n) {
  this.name = n
  this.talk = function() {
    console.log(this)
  }
}

const me = new Person('Polo')
me.talk() // Person { name: 'Polo', talk: func }
```







<h2> Description - callback functions</h2>

```javascript
function Person(n) {
  this.name = n
  this.talk = function() {
    console.log(this)
  }
  
  setTimeout(function() {
    console.log(this)
  }, 100)
}

const me = new Person('Polo')

// after 100ms, you will get the console log but this is referring to Window and not Person
// this is because the console.log is being executed in a different execution context

// Fix 1: bind
setTimeout(function() {
  console.log(this)
}.bind(this), 100)

// Fix 2: arrow function
setTimeout(() => {
  console.log(this)
}, 100)
```

