# Testing with Jest



[Testing asynchronous code with callbacks](#Async code with callbacks)

[Testing asynchronous code with promises and async/await](#Async code with promises and async/await)

[Spies and mocks - dealing with side effects](#Spies and Mocks)



## Async code with callbacks

```js
// Code to test

import jwt from 'jsonwebtoken'

export const generateToken = (userEmail, doneFn) => {
  jwt.sign({ email: userEmail }, 'somePassword', doneFn)
}
```

```js
// Test file

it('should generate a token value', (done) => {
  const testUserEmail = 'someEmail@test.com'
  
  generateToken(testUserEmail, (err, token) => {
    try {
      expect(token).toBeDefined()
      done()
    } catch (err) {
      done(err)
    }
  })
})
```



## Async code with promises and async/await

```js
// Code to test

import jwt from 'jsonwebtoken'

export const generateToken = (userEmail) => {
  const promise = new Promise((resolve, reject) => {
    jwt.sign({ email: userEmail }, 'somePassword', (error, token) => {
      if (error) {
        reject(error)
      } else {
        resolve(token)
      }
    })
  })
  
  return promise
}
```

```js
// Test file

it('should generate a token value', () => {
  const testUserEmail = 'someEmail@test.com'
  return expect(generateToken(testUserEmail).resolves.toBeDefined())
})

it('should generate a token value', async () => {
  const testUserEmail = 'someEmail@test.com'
  const token = await generateToken(testUserEmail)
  expect(token).toBeDefined()
})
```



## Spies and mocks

Spies: wrappers around functions or empty replacements for functions that allow you to track if and how (i.e. arguments) a function was called.

```js
const someMethod = jest.fn() // keeps track of function executions and arguments passed in

expect(someMethod).toBeCalled()
expect(someMethod).toBeCalledTimes(1)
expect(someMethod).toBeCalledWith(someArgs)

// jest.fn(someFunc) accepts an argument someFunc that will replace the dummy implementation
jest.fn(() => {})

// .mockImplementationOnce() and .mockImplementation()

// .mockReturnValueOnce() and .mockReturnValue()
myMock = jest.fn()
console.log(myMock) // undefined
myMock.mockReturnValueOnce(10).mockReturnValueOnce('x').mockReturnValue(true)
console.log(myMock(), myMock(), myMock(), myMock()) // 10, x, true, true
```

Mocks: replacement for an API that may provide some test-specific behaviour instead

```js
jest.mock('pathToModuleToMock') // replaces all methods with empty spy dummies

expect(pathToModuleToMock.someMethod).toBeCalled()

// Example: specifying implementation
// use default if mocking the default function returned from the module
import path from 'path'

jest.mock('path', () => {
  return {
    default: {
      join: (...args) => {
        return args(args.length - 1)
      }
    }
  }
})
```

You can manage custom mock implementations globally via `__mocks__` folder: https://jestjs.io/docs/manual-mocks





















