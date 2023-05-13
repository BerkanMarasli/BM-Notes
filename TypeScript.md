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

























