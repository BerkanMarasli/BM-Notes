# TypeScript

## Advanced TypeScript

### Intersection Types

```typescript
type Admin = {
  name: string
  privileges: string[]
}

type Employee = {
  name: string
  startDate: Date
}

// type intersection (object)
type ElevatedEmployee = Admin & Employee

const e1: ElevatedEmployee = {
  name: '',
  privileges: ['']
  startDate: new Date()
}
```

```typescript
interface Admin {
  name: string
  privileges: string[]
}

interface Employee {
  name: string
  startDate: Date
}

// interface inheritence
interface ElevatedEmployee extends Admin, Employee

const e1: ElevatedEmployee = {
  name: '',
  privileges: ['']
  startDate: new Date()
}
```

```typescript
type A = stirng | number
type B = number | boolean

// type intersection (non-object)
type C = A & B // type C = number
```

### Type Guards

- typeof
- in
- instanceof

```typescript
type A = string | number

const add = (b: A, c: A) => {
  // return b + c will not work on its own
  // The if statement is a type guard using typeof
  if (typeof b === 'string' || typeof b === 'string') {
    return a.toString() + b.toString()
  }
  return a + b
}
```

```typescript
type Admin = {
  name: string
  privileges: string[]
}

type Employee = {
  name: string
  startDate: Date
}

// type intersection (object)
type ElevatedEmployee = Admin & Employee

type UnknownEmployee = Employee | Admin

const printEmployeeInformation = (employee: UnknownEmployee) => {
  console.log('Name: ', employee.name)
  
  // down side of typeof (for non primitive types)
  if (typeof employee === 'object') { // doesnt tell us if privileges is a field within employee
    console.log('Privileges: ', employee.privileges) 
  }
  // use JavaScript's in keyword to check field within object
  if ('privileges' in employee) {
    console.log('Privileges: ', employee.privileges)
  }
  
  if ('startDate' in employee) {
    console.log('Start Date: ', employee.privileges)
  }
}
```

```typescript
class Car {
  drive() {
    console.log('Driving...')
  }
}

class Truck {
  drive() {
    console.log('Driving...')
  }
  
  loadCargo(amount: number) {
    console.log('Loading cargo ... ' + amount)
  }
}

type Vehicle = Car | Truck
const car = new Car()
const truck = new Truck()

const useVehicle = (vehicle: Vehicle) => {
  vehicle.drive()
  
  vehicle.loadCargo(1000) // error
  // use JavaScript's instanceof keyword
  if (vehicle instanceof Truck) {
     vehicle.loadCargo(1000)
  }
}
```

### Discriminated Unions

Useful when working with object and union types.

```typescript
interface Bird {
  type: 'bird'
  flyingSpeed: number
}

interface Horse {
  type: 'horse'
  runningSpeed: number
}

type Animal = Bird | Horse

const moveAnimal = (animal: Animal) => {
  console.log('Moving with speed: ' + animal.flyingSpeed) // errror
   // animal instanceof Bird will not work as at runtime, interfaces are not compiled
  
  // discriminated unions have one property in every union used to distinguish (type or kind normally)
  let speed;
  switch (animal.type) {
    case 'bird':
      speed = animal.flyingSpeed
      break
    case 'horse':
      speed = animal.runningSpeed
      break
  }
}
```

### Type Casting

Override TypeScript where it cannot figure out the type of something but you as the developer know what the type is.

- <>
- as

```typescript
const paragraph = document.querySelector('p') // HTMLParagraphElement | null

const paragraphWithId = document.getElementById('some-id')! // HTMLElement
paragraphWithId.value = '' // Error: property value does not exist on type HTMLElement

// using <>
const paragraphWithId = <HTMLParagraphElement>document.getElementById('some-id')!;

// as keyword
const paragraphWithId = document.getElementById('some-id')! as HTMLParagraphElement;
```

```typescript
// ! at the end of the expression tells TypeScript the expression will never yield null
// Its not required if Type Casting
```

### Index (Properities) Types

```typescript
// Target is an interface of { email: 'not a valid email', username: 'must start with character' }
// Do not know exactly key name and the number of key/value pairs
interface ErrorContainer {
  id: string // Can have this key/value
  count: number // Cannot have this key/value as it breaks rule below
  [key: string]: string
}
```

### Function Overloads

```typescript
type A = string | number

const add = (b: A, c: A) => {
  if (typeof b === 'string' || typeof b === 'string') {
    return a.toString() + b.toString()
  }
  return a + b
}

// type = A and not number
const result = add(1,5)

// function overloading
function add(b: number, c: number): number;
function add(b: string, c: string): string;
function add(b: number, c: string): string;
function add(b: A, c: A) {
  if (typeof b === 'string' || typeof b === 'string') {
    return a.toString() + b.toString()
  }
  return a + b
}

// Number of function overloading is not limited
// When compiling, TypeScript will combine the logic for runtime
```

### Optional Chaining

```typescript
const fetchedUserData = {
  id: '',
  name: '',
  // job: { title: '', description: ''}
}

// when the data is uncertain
fetchedUserData.job && fetchedUserData.job.title // JavaScript method
fetchedUserData?.job?.title // TypeScript method

// behind the scenes, it compiles to a if condition to check if the expression before the ? is not undefined
```

### Nullish Coalescing

```typescript
const userInput = ''

// || checks for falsy values: null, undefined, '' (empty string)
const storedData = userInput || 'DEFAULT' // storedData = 'DEFAULT'

// To check for just undefined or null ONLY
const storedData = userInput ?? 'DEFAULT' // storedData = ''
```

## Generics

### Creating A Generic Function

```typescript
// return type = object
function merge(obJA: object, objB: object) {
  return Object.assign(objA, objB)
}

// return type = T & U
function merge<T, U>(objA: T, objB: U) {
  return Object.assign(objA, objB)
}
```

### Working With Constraints

```typescript
function merge<T, U>(objA: T, objB: U) {
  return Object.assign(objA, objB)
}

const mergedObj = merge({name: '', hobbies: ['']}, 30) // cannot merge number with object

// add a type constraint using extends keyword
function merge<T extends object, U extends object>(objA: T, objB: U) {
  return Object.assign(objA, objB)
}
```

### keyof Contraint

```typescript
function extractAndConvert(obj: object, key: string) {
  return obj[key] // dont know if the object will have a key that is equal to key
}

function extractAndConvert<T extends object, U extends keyof T>(obj: T, key: U) {
  return obj[key]
}
```

### Generic Classes

```typescript
class DataStorage<T> {
  private data: T[] = []
  
  addItem(item: T) {
    this.data.push(item)
  }
  
  removeItem(item: T) {
    if (this.data.indexOf(item) === -1) {
      return
    }
    this.data.splice(this.data.indexOf(item), 1)
  }
  
  getItems() {
    return [...this.data]
  }
}

const textStorage = new DataStorage<string>() // works

const objStorage = new DataStorage<object>() // object passed in by reference which causes problem when trying to remove an object item (two different object references)
// Therefore you only really want it to work with primative values... (in this case)

class DataStorage<T extends string | number | boolean> {}
```

### Generic Utility Types

https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype

```typescript
// Partial utility type
interface CourseGoal {
  title: string
  description: string
  completeUntil: Date
}

const createCourseGoal = (title: string, description: string, date: Date): CourseGoal => {
  let courseGoal: Partial<CourseGoal> = {} // Partial wraps our type and makes all properities as optional
  courseGoal.title = title
  courseGoal.description = description
  courseGoal.completeUntil = completeUntil
  return courseGoal as CourseGoal
}

// Readonly utility type
const names: Readonly<string[]> = ['', '']
names.push('') // TypeScript error
```























