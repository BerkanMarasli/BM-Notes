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



### Type Casting

### Function Overloads

