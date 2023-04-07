**Component testing**

**TestBed**

In order to avoid having to manually create an instance of the component, resolve and inject dependencies and set inputs, the Angular team provides the TestBed to ease unit testing. The TestBed creates and configures an Angular environment so you can test particular parts like components and services safely and easily.

The TestBed comes with a testing module where you can declare components, directives and pipes, provide services and other injectables as well as import other modules. TestBed has a static method configureTestingModule that accepts a module definition. Once configured, to compile all declared components, directives and pipes we use the method compileComponents.

```
// Common Angular TestBed pattern

TestBed.configureTestingModule({
 imports: [],
 declarations: [],
 providers: [],
}).compileComponents()
// Example: 

TestBed.configureTestingModule({
 declarations: [ComponentToTest]
}).compileComponents()
```

Additionally, compileComponents is an asynchronous operation as Angular needs to fetch external template files referenced by templateUrl. Therefore it needs to be wrapped around async/await.

 

**Rendering the component**

Use TestBed's static method createComponent to render the component under test. createComponent returns a ComponentFixture (a wrapper around the component with useful testing tools).

```
// Example

const fixture = TestBed.createComponent(ComponentToTest)
```

createComponent renders the component into a div container element in the DOM however the component is not fully rendered. All the static HTML is present but the dynamic HTML is missing (e.g. template bindings like {{ taxYear }} are not evaluated.

We have to trigger the change detection manually. This is a purpose built feature in testing as it allows us to test asynchronous behaviour in a synchronous manner.

Use the fixture's detectChanges method to trigger detection manually.

```
// Example

fixture.detectChanges()
```

 

**Complete test setup in Jest test suite**

```
describe('ComponentToTest', () => {
 let fixture: ComponentFixture<ComponentToTest>

 beforeEach(async () => {
  await TestBed.configureTestingModule({
   declarations: [ComponentToTest]
  }).compileComponents()

  fixture = TestBed.createComponent(ComponentToTest)
  fixture.detectChanges()
 })
})
```

 

------

 

**ComponentFixture and DebugElement**

In Angular, the ComponentFixture holds the component and provides a convenient interface to both the component instance and the rendered DOM.

The fixture references the component instance via the componentInstance property. The component instance is mainly used to set inputs and subscribe to outputs.

```
// Example

const component = fixture.componentInstance
```

For accessing elements in the DOM, Angular uses the abstraction DebugElement which wraps the native DOM element. The fixture's debugElement property returns the component's host element (identified by the component's selector).

The DebugElement offers properties like properties, attributes, classes, styles to examine the DOM element itself. The properties parent, children, childNodes help navigating in the DOM tree and return a DebugElement as well.

```
// Example

const { debugElement } = fixture
```

Often it is necessary to unwrap the DebugElement to access the native DOM element inside. nativeElement is typed as any because Angular does not know the exact type of the wrapped DOM element (most of the time it is a subclass of HTMLElement). When using nativeElement, use the DOM interface of the specific element. For example, a button element is represented as HTMLButtonElement in the DOM.

```
// Example

const { debugElement } = fixture
const { nativeElement } = debugElement

// Access properties:
console.log(nativeElement.tagName);
console.log(nativeElement.textContent);
console.log(nativeElement.innerHTML);
```

 

**Complete test setup in Jest suite**

```
describe('ComponentToTest', () => {
 let fixture: ComponentFixture<ComponentToTest>
 let debugElement: DebugElement

 beforeEach(async () => {
  await TestBed.configureTestingModule({
   declarations: [ComponentToTest]
  }).compileComponents()

  fixture = TestBed.createComponent(ComponentToTest)
  fixture.detectChanges()
  debugElement = fixture.debugElement
 })

 it('some test', () => {})
}) 
```

 

------

 

**Querying the DOM with ids**

Every DebugElement features the methods query and queryAll for finding descendant elements:

- query returns the first descendant element that meets a condition
- queryAll returns an array of all matching elements

Both methods expect a function (predicate) judging every element and returning true or false.

You can use By.css as the predicate.

The return value of query is a DebugElement, that of queryAll is an array of DebugElements.

```
// Example

const { debugElement } = fixture

// Find the first h1 element
const h1 = debugElement.query(By.css('h1'))

// Find all elements with the class .user
const userElements = debugElement.queryAll(By.css('.user'))

// Find the first element with the data-testid attribute user
const userElement = debugElement.query(
 By.css('[data-testid="user"]')
)
```

 

**Triggering event handlers**

DebugElement has a useful method for firing events: triggerEventHandler. This method calls all event handlers for a given event type like click. As a second parameter, it expects a fake event object that is passed to the handlers.

```
// Example

// Trigger button click without event object: (click)="increment()"
button.triggerEventHandler('click', null)

// Trigger button click with event object: (click)="increment(someObject)"
button.triggerEventHandler('click', {
 /* ... Event properties ... */
})
```

This works if the event handler is registered on the element itself. If the event handler is registered on a parent and relies on event bubbling, you need to call triggerEventHandler directly on that parent. triggerEventHandler does not simulate event bubbling or any other effect a real event might have.

IMPORTANT: Angular does not automatically detect changes in order to update the DOM therefore you need to trigger the change detection manually (i.e. re-render the component).

 

**Expecting text output**

The DebugElement does not have a method or property for reading the text content therefore we need to access the native DOM element that has the property.

```
// Example

h1Tag.nativeElement.textContent
```

 

------

 

**Testing inputs**

An input is a special property of the component instance and can be set via the property. Inputs should be set in the beforeEach block, before calling detectChanges.

```
// Example

const component = fixture.componentInstance
component.someInputVar = 0
```

Inputs should not be manipulated by the component, but simply read.

 

**Component lifecycle methods**

If a state or property is set in a component lifecycle method (e.g. ngOnChanges), you need to call the lifecycle method in the beforeEach block, before calling detectChanges.

 

**Testing outputs**

Outputs send data from child to parent component (opposite to input's functionality).

```
// Example

class Component {
 @Output()
 public countChange = new EventEmitter<number>()
}
```

EventEmitter is a subclass of RxJS Subject, which itself extends RxJS Observable. The component uses the emit method to publish new values. The parent component uses the subscribe method to listen for emitted values. In the testing environment, we do the same.

```
// Example

it('emits countChange events on increment', () => {
 let actualCount: number | undefined
 component.countChange.subscribe((count: number) => {
  actualCount = count
 })

 click(fixture, 'increment-button') // Helper function for click event

 expect(actualCount).toBe(1)
})
```

 

------

 

**Testing components with children components**

When a component contains other, children components; you can:

1. Either declare the child components in the testing module. This turns the test into an integration test.
2. Or tell Angular to ignore the known elements. This turns the test into a unit test.

**Unit test**

When configuring the testing module, we can specify schemas to tell Angular how to deal with elements that are not handled by directives or components. If web components then CUSTOM_ELEMENTS_SCHMEA but if we want Angular to simply ignore the elements then NO_ERRORS_SCHEMA.

```
// Example

await TestBed.configureTestingModule({
 declarations: [ParentComponent],
 schemas: [NO_ERRORS_SCHEMA]
})
```

Then we can simply use the DebugElement to query by css for child component's selector to test if the component renders. A generic helper function 'findComponent<T>' could be useful here.

You can test the input to a component by querying for the component and using the properties property to access the key. For example:

```
// Example

<app-counter
  [startCount]="5"
  (countChange)="handleCountChange($event)"
></app-counter>

it('passes a start count', () => {
  const counter = findComponent(fixture, 'app-counter');
  expect(counter.properties.startCount).toBe(5);
}); 
```

You can test the output by treating it as an event (from the parent components point of view). There is no instance of the child component and no EventEmitter. You need to simulate the output using the known triggerEventHandler method. The child component is found and the event is triggered with the value of the emitted output as the second parameter.

```
// Example

<app-counter [startCount]="5" (countChange)="handleCountChange($event)"
></app-counter>

it('listens for count changes', () => {
  /* … */
  const counter = findComponent(fixture, 'app-counter');
  const count = 5;
  counter.triggerEventHandler('countChange', 5);
  /* … */
});
```

 

**Integration test (rendering the child components)**

- Creating fake component(s) for the children - not covered
- Faking a child component with ng-mocks - covered

ng-mocks is a feature-rich library for testing components with fake (a.k.a mock or stub) dependencies.

The MockComponent function expects the original component and returns a fake that resembles the original. This is added to the testing module.

 

```
// Example

import { MockComponent } from 'ng-mocks'

beforeEach(async () => {
  await TestBed.configureTestingModule({
    declarations: [ComponentToTest, MockComponent(ChildComponent)],
    schemas: [NO_ERRORS_SCHEMA]
  }).compileComponents()
})
```

We can then query the rendered DOM for an instance of ChildComponent. The found instance is in fact a fake created by ng-mocks. The fake has all properties and methods that the original component has.

```
// Example

describe('ParentComponent with ng-mocks', () => {
  let fixture: ComponentFixture<ParentComponent>
  let component: ParentComponent
  let counter: ChildComponent

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [ParentComponent, MockComponent(ChildComponent)],
      schemas: [NO_ERRORS_SCHEMA]
    }).compileComponents()

    fixture = TestBed.createComponent(ParentComponent)
    component = fixture.componentInstance
    fixture.detectChanges()

    const counterElement = fixture.debugElement.query(
      By.directive(ChildComponent)
    )
    counter = counterElement.componentInstance;
  })

  it('renders an independent counter', () => {
    expect(counter).toBeTruthy()
  })

  /* … */
}) 
```

 

------

 

**Testing components depending on services**

Angular's dependency injection maintains only one app-side instance of a service, a so-called singleton. Therefore, multiple instances of a component share the same state if the service is being used to store state.

Two functional ways to test:

- A unit test that replaces the service dependency with a fake
- An integration test that includes a real service

**Service dependency integration test**

For simple services with little logic and no further dependencies or side effects like HTTP requests, we can simply provide the actual service in the TestBed providers property.

Angular creates an instance of the service and injects it into the component under test.

```
// Example

await TestBed.configureTestingModule({
 declarations: [ComponentToTest],
 providers: [ServiceToTest]
}).compileComponents()
```

In order to test if the state in the service is being used, you could create two instances of the component and check the results match.

**Fake service dependency**

Mocking a service is actually extremely difficult (with no set standard) and involves a lot of boilerplate.

 

 

 

 