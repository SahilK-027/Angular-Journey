# Angular Journey SK027

## How Angular project works ?

### Create Angular Project and start development

- Creates a new Angular workspace.

```bash
ng new [project_name]  # [aliases: n]
```

- Builds and serves your application, rebuilding on file changes.

```bash
ng serve [project]  #[aliases: s]
```

- Theory

Angular CLI saves the compile Angular application in the memory & directly starts it. If we make any changes to our Angular app, Angular CLI will recompile & update the file. Angular CLI uses Webpack to traverse through our Angular app & it bundles IS & other files into one or more bundles. Then Angular CLI also injects the bundled JavaScript & CSS files in the index.html.

When the index.html file is loaded, Angular core libraries & third party libraries are also loaded by that time. Now Angular needs to locate the main entry point. So it analyzes angular.json (get the main file).

```json
"options":{
    "outputPath": "dist/angular-ekart",
    "index": "src/index.html",
    "main": "src/main.ts",
    "polyfills": ["zone.js"]
}
```

- Flow
  Angular Project -> index.html -> angular.json (get the main file) -> main.ts -> AppModule -> AppComponent -> View Template(app. component. html)

## Types of Component Selector

- Like HTML tag: selector : "app-nav"
- Like HTML attribute: selector : "[app-nav]"
- Like HTML class: selector : ".app-nav"

## Data Binding

Data Binding in Angular allows us to communicate between a component class (Typescript) and its corresponding view template (Html) & vice-versa.

Component <---------------------------------> View Template
(UI logic) (HTML Page)

- Example: How data binding is done in between component and template
- Component UI logic

```typescript
export class MyComponent{
    title = "dummy title';
    message = 'SK LEARNS ANGULAR';
    display = false;
    onclick(){
        this.display = true;
    }
}
```

- View Template

```html
<div class="header">
  <div>{{ title }}</div>
  <div>{{ message }}</div>
  <button （click）="onclick()">Click Me</button>
  <div [hidden]="display">
    <p>Display this content</p>
  </div>
</div>
```

### Advantages and disadvantages of angular over react.
Angular: 
- Two-way data binding by default using ngModel.
- Angular's built-in dependency injection system makes it easier to manage and organize components, services, and other application parts.
- Angular applications may have larger initial bundle sizes compared to React, which can impact page load times. However, Angular provides tools to optimize and reduce bundle sizes.
- More strict and better error handling.

Rect: Unidirectional data flow, but supports two-way binding using state management libraries (e.g., Redux).

### Types of Data Binding in Angular

- Type 1: One Way Data Binding
  Component to View OR View to Component

One-way data binding is when, data can be access from component into its corresponding view or vice versa.

    - Type 1.1: Component to View
                  Data flow from component to class view template
        Component ------------------------------------------------------------------> View Template
                  String interpolation: {{data}}
                  Property binding: [property] = data

    - Type 1.2: View to Component
                    Data flow from view template to component class
        Component <------------------------------------------------------------------ View Template
                  Event binding:: (data)="expression"

#### String interpolation

Example:

```typescript
export class ProductListComponent {
  product = {
    name: "iPhone XR",
    price: 789,
    color: "Black",
    discount: 5.5,
  };

  getDiscountedPrice(): number {
    return (
      this.product.price - (this.product.price * this.product.discount) / 100
    );
  }
}
```

```html
<p>Product name: {{product.name}}</p>
<p>Original price: {{product.price}}</p>
<p>Product color: {{product.color}}</p>
<p>Discounted price: {{getDiscountedPrice()}}</p>
```

#### Property Binding

Property binding lets us bind a property of a DOM object, for example the hidden property, to some data value. This can let us show or hide a DOM element, or manipulate the DOM in some other way.

Example:

```typescript
export class ProductListComponent {
  product = {
    name: "iPhone XR",
    price: 789,
    color: "Black",
    discount: 5.5,
    pImage: "/assets/images/iphoneXR.png",
  };

  getDiscountedPrice(): number {
    return (
      this.product.price - (this.product.price * this.product.discount) / 100
    );
  }
}
```

```html
<!-- we can do it with string interpolation like this -->
<!-- <img src = {{product.pImage}}> -->

<!-- we can do it with property binding like this
Inside "" you can write any typescript expression -->
<img [src]="product.pImage" />
<img bind-src="product.pImage" />
<!-- Another syntax for property binding -->

<p>Product name: {{product.name}}</p>
<p>Original price: {{product.price}}</p>
<p>Product color: {{product.color}}</p>
<p>Discounted price: {{getDiscountedPrice()}}</p>
```

Note: If we can do same stuff with string interpolation why do we need property binding?
Ans: For html attributes like hidden, Disabled, Checked we need to use property binding only there string interpolation won't work.

#### Event Binding

In Angular, event binding is a mechanism that allows you to respond to user events, such as clicks, key presses, mouse movements, etc. It's a way to capture and handle events raised by the user or by other components in your application.

For event binding wrap the event with (). Angular supports a variety of events such as (click), (keyup), (change), etc

In Angular, the (change) event binding is used to capture and respond to changes in an input element, such as text inputs, checkboxes, and select dropdowns. The (change) event is triggered when the user interacts with the input, and the value of the input changes.

- Example

```typescript
export class AppComponent {
  // Initial value for the dynamicName
  dynamicName: string = "Not entered name";

  // Method to be called on input change
  onNameChange(newName: string): void {
    // Update dynamicName with the new value
    this.dynamicName = newName;
  }
}
```

```html
<!-- Input tag with event binding for input event -->
<input
  placeholder="Enter your name"
  (input)="onNameChange($event.target.value)"
/>

<!-- Paragraph with string interpolation -->
<p>{{ dynamicName }}</p>
```

- Type 2: `Two Way Data Binding`
  Component to View View to Component

Two-way data binding binds data from component class to view template and view template to component class. It is a combination of property binding & event binding.

Two-way data binding in Angular is commonly achieved using [(ngModel)]. `ngModel` is the directive that provides two-way data binding in Angular forms.

Ensure that you have the FormsModule imported in your Angular module. The FormsModule is required for using ngModel for two-way data binding.

- Use [(ngModel)] in HTML:

You can use [(ngModel)] within your template to create two-way data binding. It's often used in conjunction with form controls like input elements.

```html
<!-- app.component.html -->

<input [(ngModel)]="username" placeholder="Enter your username" />

<!-- The value of the input is bound to the 'username' property of the component -->
```

- Use in component

In your component TypeScript file, you should have a property (e.g., username) that you bind to in the template.

```typescript
// app.component.ts

import { Component } from "@angular/core";

@Component({
  selector: "app-root",
  templateUrl: "./app.component.html",
  styleUrls: ["./app.component.css"],
})
export class AppComponent {
  // The 'username' property is bound to the input value using [(ngModel)]
  username: string = "";
}
```

#### Property Binding + Event Binding

Two-way data binding in Angular is a combination of property binding and event binding. It allows data to flow in both directions: from the component class to the view template (property binding) and from the view template to the component class (event binding). This is often used with [(ngModel)], which is a built-in directive in Angular.

Here's an example demonstrating two-way data binding:

```typescript
export class AppComponent {
  // Initial value for the dynamicName
  dynamicName: string = "Not entered name";

  // Method to be called on input change
  onNameChange(newName: string): void {
    // Update dynamicName with the new value
    this.dynamicName = newName;
  }
}
```

```html
<!-- Input tag with event binding for input event -->
<!-- one way
<input
  placeholder="Enter your name"
  (input)="onNameChange($event.target.value)"
/> -->

<!-- Or the other -->
<input placeholder="Enter your name" [(ngModel)]="dynamicName" />

<!-- Paragraph with string interpolation -->
<p>{{ dynamicName }}</p>
```

[(ngModel)]="dynamicName" is Angular's two-way data binding syntax. It binds the value of the input element to the dynamicName property in the component class.

As the user types in the input field, the dynamicName property is updated automatically, and the paragraph below reflects the changes instantly.

Two-way data binding is a powerful feature in Angular as it simplifies the synchronization of data between the component and the view. It enhances the developer experience by reducing the need for explicit event handling for input changes. However, it's essential to note that for two-way data binding to work, you need to import the FormsModule from @angular/forms in your Angular module.

## Angular Directives

In Angular, directives are a category of functionalities that allow you to extend the behavior of HTML elements or create reusable components. They are markers on a DOM element that tell Angular to do something to that element or its content. Angular provides several built-in directives, and you can also create custom directives.

1. Component Directives:

Component: The most common type of directive. It allows you to create reusable, encapsulated components with their own logic, templates, and styles. Components are the building blocks of Angular applications.

2.  Structural Directives:

    - \*ngFor
      ngFor is a structural directive in Angular that is used for rendering a template for each item in a collection. It's commonly used for iterating over arrays or lists to generate dynamic content in the view.

    - Example:
      Simple

    ```html
    <div *ngFor="let fruit of ['Apple', 'Mango', 'orange', 'grapes']">
      <p>Fruit name: {{ fruit }}</p>
    </div>
    ```

    TypeScript + HTML

    ```typescript
    export class AppComponent {
      // Initial value for the dynamicName
      listOfFruits: string[] = ["Apple", "Mango", "orange", "grapes"];
    }
    ```

    ```html
    <div *ngFor="let fruit of listOfFruits">
      <p>Fruit name: {{ fruit }}</p>
    </div>
    ```

    - *ngIf
      *ngIf directive, it is a structural directive in Angular. This directive is used to conditionally render or remove elements from the DOM (Document Object Model) based on a certain expression's truthiness.

      Here's a basic example of how you might use `*ngIf` in an Angular component

      - Simple example

      ```html
      <div *ngIf="isConditionTrue">
        This content will only be displayed if isConditionTrue is true.
      </div>
      ```

      You can also use \*ngIf with an "else" clause:

      ```html
      <div *ngIf="isConditionTrue; else elseBlock">
        This content will be displayed if isConditionTrue is true.
      </div>
      <ng-template #elseBlock>
        This content will be displayed if isConditionTrue is false.
      </ng-template>
      ```

3.  Attribute Directives:

    - [ngStyle]

    The ngStyle directive in Angular is another structural directive that allows you to dynamically set inline styles for HTML elements based on expressions in your component. It gives you the flexibility to conditionally apply styles depending on certain conditions or dynamic data.

        - Example:

        ```html
        <!-- app.component.html -->
        <div [ngStyle]="{ 'color': isConditionTrue ? 'green' : 'red' }">
        {{isConditionTrue ? 'True' : 'False'}}
        </div>
        ```

    - [ngClass]

    The [ngClass] directive in Angular is used to conditionally apply CSS classes to HTML elements based on certain conditions or expressions. It provides a way to dynamically manage the classes of an element.

        - Example:

        ```html
        <!-- app.component.html -->
        <button
        [ngClass]="{ 'disabled-button': isButtonDisabled }"
        [disabled]="isButtonDisabled"
        >
        Click me
        </button>
        ```

        ```typescript
        export class AppComponent {
            isButtonDisabled: boolean = false;

            // Some logic to determine when the button should be disabled
            updateButtonState() {
                // Example: Disable the button if a condition is met
                this.isButtonDisabled = /* some condition */;
            }
        }

    ```

    In this example:

    - If isButtonDisabled is true, the CSS class disabled-button will be applied to the button, providing a visual indication that it is disabled.
    - The [disabled] attribute is bound to the isButtonDisabled variable, so if isButtonDisabled is true, the button will be disabled; otherwise, it will be enabled.

    ```

4.  Custom Directives:

You can create your own custom directives to encapsulate behavior and reuse it across your application.

## Custom Property binding

### Parent component to child component communication (use @Input() decorator)

#### @Input() Decorator

                    Custom property binding

Parent Component ---------------------------------------------------> Child Compo
@Input() decorator

Parent Compo

```html
<parent *ngFor="let fruit of listOfFruits" [fruit]="fruit"> </parent>
```

Child Compo

```typescript
export class ChildComponent {
  // Initial value for the dynamicName
  @input() fruit;
}
```

### Child component to parent component communication (use @output())

In Angular, you can achieve communication from a child component to a parent component using the @Output() decorator along with an EventEmitter. This allows the child component to emit events that the parent component can listen to.

- Example

Child Component (child.component.ts)

```typescript
import { Component, EventEmitter, Output } from "@angular/core";

@Component({
  selector: "app-child",
  template: ` <button (click)="sendMessage()">Send Message to Parent</button> `,
})
export class ChildComponent {
  @Output() messageEvent = new EventEmitter<string>();

  sendMessage() {
    const message = "Hello from the child component!";
    this.messageEvent.emit(message);
  }
}
```

- The @Output() decorator is used to create an EventEmitter named messageEvent. This EventEmitter will emit events that the parent component can subscribe to.
- The sendMessage() method is triggered when the button is clicked. It emits a message using this.messageEvent.emit(message).

Parent Component (parent.component.ts)

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "app-parent",
  template: `
    <app-child (messageEvent)="receiveMessage($event)"></app-child>
    <p>{{ receivedMessage }}</p>
  `,
})
export class ParentComponent {
  receivedMessage: string = "";

  receiveMessage(message: string) {
    this.receivedMessage = message;
  }
}
```

- The ParentComponent listens for the (messageEvent) emitted by the ChildComponent.
- When the event is received, the receiveMessage($event) method is called, which updates the receivedMessage property in the parent component.

### Communication between non related components

<img width="1437" alt="Screenshot 2023-12-02 at 7 15 20 PM" src="https://github.com/SahilK-027/Angular-Journey/assets/104154041/cfe6b846-0d42-47a6-87b2-e3a800ac7494">

> **Note**
> We can achieve this by much simpler way using Services in Angular

## Template reference variables

Template reference variables in Angular allow you to create a reference to an element in the template and then use that reference in the component code. They are denoted by a hash symbol (#) followed by a name.

Template reference variables in Angular allow you to create a reference to an element in the template and then use that reference in the component code. They are denoted by a hash symbol (#) followed by a name.

Here's a basic example:

```html
<!-- app.component.html -->
<input type="text" #myInput />
<button (click)="logInputValue(myInput.value)">Log Input Value</button>
```

In this example:

- #myInput is a template reference variable assigned to the input element.
- When the button is clicked, the (click) event triggers the logInputValue method in the component, passing the value of the input using myInput.value.

## Components vs Directives

| Feature              | Components                                                       | Directives                                  |
| -------------------- | ---------------------------------------------------------------- | ------------------------------------------- |
| **Definition**       | Represent a UI control or a view                                 | Extend the behavior of elements in the DOM  |
| **Encapsulation**    | Have their own template, styles, and logic                       | Do not have their own template or styles    |
| **Usage**            | Used as custom elements in templates                             | Used to change the behavior of elements     |
| **Creation**         | Created using the `@Component` decorator                         | Created using the `@Directive` decorator    |
| **Metadata**         | Use `@Component` metadata                                        | Use `@Directive` metadata                   |
| **Selector**         | Identified by an HTML element selector                           | Identified by an attribute selector         |
| **Template**         | Has its own template                                             | Can modify the template of the host element |
| **Example**          | `<app-example></app-example>`                                    | `<div appExample></div>`                    |
| **Controller/Class** | Has a class with properties and methods                          | Has a class with properties and methods     |
| **Purpose**          | Encapsulate a behavior or feature                                | Add behavior or manipulate DOM elements     |
| **Example Use Case** | Creating a login form component                                  | Creating a custom validation directive      |
| **Two-Way Binding**  | Supports two-way data binding (via `ngModel`)                    | Typically used for one-way binding          |
| **Lifecycle Hooks**  | Has a set of lifecycle hooks (e.g., `ngOnInit`, `ngOnChanges`)   | Can use lifecycle hooks but has fewer       |
| **Communication**    | Can communicate with other components through services or events | Often used for DOM manipulation or styling  |

## @ViewChild() in Angular

The ViewChild decorator is used to query and get a reference of the DOM element in the component. It returns the first matching element.

Apply the @ViewChild decorator to a property in your component class.

```typescript
@ViewChild(SomeComponent) someComponentRef: SomeComponent;
```

## ng-template

```html
<h2>Learn NG Template</h2>
<ng-template #myTemplate>
<h3>This is a template</h3>
‹p›This is an example paragraph to understand ng-template</p>
</ng-template>

‹!--ngTemplate@Outlet Directive--›
<div *ngTemplateOutlet="myTemplate"></₫iv> I
```

In this example, SomeComponent is the type of the child component you want to reference, and someComponentRef is the property that will hold the reference to the child component.

## Lifecycle Hooks in Angular

In Angular, lifecycle hooks are methods that allow you to tap into the lifecycle of a component, directive, or service. These hooks provide points in the lifecycle where you can perform operations or respond to changes.

### The change detection cycle

In Angular, change detection is a mechanism that ensures the view reflects the current state of the application data. The change detection cycle is the process by which Angular determines what changes have occurred in the application state and updates the view accordingly. It's an essential part of Angular's architecture to maintain a responsive and synchronized user interface.

- When change detection occurs?
  - Whenever the @input property of a component changes
  - Whenever a DOM event happens. E.g. Click or Change
  - Whenever a timer events happens using setTimeOut () OR setInterval().
  - Whenever an HTTP request is made.

#### ngOnChanges Lifecycle Hook:

In Angular, the ngOnChanges lifecycle hook is part of the lifecycle hooks that a component can implement. This hook is called whenever there is a change in the `input properties` of the component. It provides a way for the component to respond to changes in its input properties and take appropriate actions.

- Ex.

```typescript
import { Component, Input, OnChanges, SimpleChanges } from "@angular/core";

@Component({
  selector: "app-example",
  template: "<p>{{ inputData }}</p>",
})
export class ExampleComponent implements OnChanges {
  @Input() inputData: string;

  ngOnChanges(changes: SimpleChanges): void {
    if (changes.inputData) {
      console.log("InputData changed:", changes.inputData);
      // Perform actions based on the changes to inputData
    }
  }
}
```

#### ngOnInit Lifecycle Hook:

The ngOnInit lifecycle hook in Angular is a method that is called after the Angular component has been initialized. It is part of the Angular component lifecycle and provides a hook for developers to perform initialization logic for the component.

-Example

```typescript
import { Component, OnInit } from "@angular/core";

@Component({
  selector: "app-example",
  template: "<p>{{ message }}</p>",
})
export class ExampleComponent implements OnInit {
  message: string;

  ngOnInit(): void {
    this.message = "Component initialized";
    // Perform other initialization tasks here
  }
}
```

In the example above, the ngOnInit hook is used to set the message property of the component to 'Component initialized'. This is a simple example, but in a real-world scenario, you might use this hook to make HTTP requests, set up subscriptions, or perform other tasks necessary for the component's operation.

The ngOnInit hook is called once after the component is created, making it a suitable place for tasks that should happen only once during the component's lifecycle. Keep in mind that it is called after the constructor but before the ngOnChanges hook.

#### ngDoCheck Lifecycle Hook:

The ngDoCheck lifecycle hook in Angular provides a mechanism for developers to implement custom change detection for a component. Unlike other lifecycle hooks, ngDoCheck is called during every change detection cycle, giving you an opportunity to check for changes and perform custom logic.

-Example:

```typescript
import { Component, DoCheck } from "@angular/core";

@Component({
  selector: "app-example",
  template: "<p>{{ data }}</p>",
})
export class ExampleComponent implements DoCheck {
  data: string = "Initial Data";

  ngDoCheck(): void {
    // Perform custom change detection logic
    console.log("ngDoCheck triggered");
  }
}
```

Keep in mind that using ngDoCheck requires careful consideration, as it is called frequently. Overusing or performing heavy operations in this hook can have performance implications. It is often used in conjunction with the ChangeDetectorRef service to manually trigger change detection when needed.

Typically, for most scenarios, Angular's default change detection is sufficient. However, ngDoCheck provides a way to implement custom change detection logic when necessary.

### Lifecycle hooks sequence

<div align="center">
<img width="472" alt="Screenshot 2023-12-03 at 11 02 00 AM" src="https://github.com/SahilK-027/Angular-Journey/assets/104154041/ecf440e1-02d5-4092-915b-27641dcc8161">
</div>

## Services

In Angular, services are a way to organize and share code across components. They are singleton objects that can be injected into components, directives, or other services to provide functionality that is independent of any particular component. Let's explore some real-life use cases for Angular services:

1. Data Sharing and Communication:

- Use Case: Imagine you have a shopping cart component and a product list component. When a user adds a product to the cart, you need a way to update the cart information in real-time and reflect those changes in the product list.
- Service Role: You can create a CartService that holds the cart data and provides methods for updating and retrieving the cart information. Both the shopping cart component and the product list component can inject and use this service to communicate and share data.

2. HTTP Requests and API Integration:

- Use Case: When you need to interact with a backend API to fetch or send data, you don't want to handle HTTP requests directly in your components. You want a centralized service to manage API calls.
- Service Role: Create an ApiService that encapsulates the HTTP requests. Components can then inject and use this service to make API calls, and the service can handle error handling, request/response transformations, and other related tasks.

3. User Authentication:

- Use Case: Implementing user authentication involves handling login, logout, and checking the user's authentication status across multiple components.
- Service Role: Create an AuthService that manages user authentication state. Components can use methods like login, logout, and isLoggedIn provided by this service. The service can also emit events when the authentication state changes.

4. Caching and State Management:

- Use Case: Suppose you have an application that fetches a large dataset from the server, and you want to cache this data locally to improve performance and reduce redundant server calls.
- Service Role: Create a CacheService that stores and retrieves data. Components can use this service to check if data is already available locally before making a new request to the server. This helps in optimizing the application's performance.

5. Global Event Handling:

- Use Case: You might need a way for components to communicate indirectly, for example, to trigger a specific action when a user interacts with one component and have another component respond to that action.
- Service Role: Create an EventService that allows components to publish and subscribe to events. Components can use this service to announce events, and other components interested in those events can subscribe to them.

Below is a simple example of an Angular service that handles click events and logs a message to the console. In this example, we won't use dependency injection, and the service will be a plain JavaScript object.

```typescript
// subscribe-event.service.ts

export class SubscribeService = {
  onSubscribeClick(){
    console.log('Thank you for subscription');
  }
};
```

This service is a simple JavaScript object (SubscribeService) iyt contains onSubscribeClick method to log a message to the console whenever a click occurs.

You can use this service in your components like this:

```typescript
// example.component.ts

import { SubscribeService } from "./click-event.service";

export class ExampleComponent {
  onSubscribeClick() {
    let subService = new SubscribeService();
    subService.onSubscribeClick();
  }
}
```

In your template:

```html
<!-- example.component.html -->
<button (click)="onSubscribeClick()">Click me</button>
```

Note: This example is intentionally simple and doesn't follow Angular's recommended practices for creating services using dependency injection. In a real Angular application, you would typically use Angular's dependency injection system to create services for better maintainability, testability, and flexibility.

## Disadvantages of using simple Services & (Why to use Dependency Injection?)

- Without dependency injection, a class (component) is tightly coupled with its dependency (service). This makes a class non-flexible. Any change in dependency forces us to change the class implementation.

- It makes testing of class difficult. Because if the dependency changes, the class has to change. And when the class changes, the unit test mock code also has to change.

- How to do the same above thing using Dependency Injection?

You can use the service in your components like this:

```typescript
// example.component.ts

import { SubscribeService } from "./click-event.service";

@Component({
  selector: "",
  templateUrl: "",

  // What to provide?
  providers: [SubscribeService],
})
export class ExampleComponent {
  // How to provide dependency
  constructor(private subService: SubscribeService) {}
  onSubscribeClick() {
    this.subService.onSubscribeClick();
  }
}
```

# Angular testing with Jasmine Framework

Jasmine is used to write the actual test cases for your Angular components, services, and other code.

Karma is responsible for running these tests in a controlled environment, such as real browsers or headless browsers, and reporting the results.

In the context of testing, a "specification" and a "test suite" are terms often associated with testing frameworks like Jasmine. Let's define each term:

## Specification

A specification refers to a single unit test or test case that describes a specific behavior or functionality of your code. A specification is typically defined using the it function in Jasmine.

```typescript
describe("MyComponent", () => {
  // It block represents a specification
  it("should do something when a condition is met", () => {});

  it("should handle another scenario correctly", () => {});
});
```

## Test Suite

A test suite is a collection of related test specifications or test cases. It is created using the describe function in Jasmine. A test suite helps organize and group related tests, making it easier to manage and run them.

```typescript
// Suit 1
describe("MyComponent", () => {
  it("should do something when a condition is met", () => {
    // Test code goes here
  });

  it("should handle another scenario correctly", () => {
    // Test code goes here
  });
});

// Suit 2
describe("AnotherComponent", () => {
  it("should have a default value", () => {
    // Test code goes here
  });

  it("should handle a specific event", () => {
    // Test code goes here
  });
});
```

Note: How to create a component without spec file

```bash
ng g c Component --skip-tests
```

## Simple Test In Jasmine

```typescript
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class CalcService {
  multiply(a: number, b: number): number {
    return a * b;
  }
}
```

```typescript
describe("Testing CalcService", () => {
  it("Should correctly multiply two numbers", () => {
    const calc = new CalcService();
    const result = calc.multiply(3, 4);
    expect(result).toBe(3 * 4);
  });
});
```

## spyOn

In Jasmine, the `spyOn` method is used to create spies, which are a way to mock or `spy on the behavior of functions, methods, or properties in your code` during testing. Spies are particularly useful for verifying that certain functions are called, checking the number of times they are called, and capturing the arguments passed to them. In the context of Angular testing, spyOn is commonly used with services, component methods, and external dependencies.

- Syntax:

```typescript
spyOn(object, methodName);
```

object: The object that contains the method you want to spy on.
methodName: The name of the method you want to spy on.

Checking Method Arguments:

Use `toHaveBeenCalledWith` to verify that a method was called with specific arguments.

As spyOn takes two arguments one for object and other for method, for passing object we need to instantiate it and while instantiating the object the constructor is called automatically. So, to avoid calling the constructor we must avoid creating the actual instance of the object to pass to the spyOn method. How can we do that?

Using `jasmine.createSpyObj("service/component", ["method"]);`

## BeforeEach in jasmine

In Jasmine, beforeEach is a function provided by the testing framework to set up preconditions or shared state before each test spec (it block) is executed. This is particularly useful for reducing redundancy in your test suite and ensuring a consistent starting point for each test.

```typescript
describe("My test suite", () => {
  let sharedVariable;

  // beforeEach is used to set up preconditions before each it block
  beforeEach(() => {
    // Initialization or setup code goes here
    sharedVariable = 10;
  });

  it("should do something", () => {
    // Test code that uses the sharedVariable
    expect(sharedVariable).toBe(10);
  });

  it("should do something else", () => {
    // Test code that also uses the sharedVariable
    sharedVariable = 20;
    expect(sharedVariable).toBe(20);
  });
});
```

You can use beforeEach to perform various setup tasks, such as initializing variables, creating objects, or setting up a test environment. It ensures that the specified code is executed before each test in the suite.

`TestBud` : TestBed is a utility in Angular's testing infrastructure that is used to configure and create instances of components, services, and other Angular elements within a testing environment. It provides a testing module environment where you can configure the dependencies and settings for testing Angular components, services, and directives.

```typescript
TestBed.configureTestingModule({
  declarations: [MyComponent],
  providers: [MyService],
});
```
