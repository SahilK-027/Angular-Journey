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

When the index.html file is loaded, Angular core libraries & third party libraries are also loaded by that time. Now Angular needs to locate the main entry point.

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

- Type 2: Two Way Data Binding
  Component to View View to Component

Two-way data binding binds data from component class to view template and view template to component class. It is a combination of property binding & event binding.

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

### Parent component to child component communication (use @input())

                    Custom property binding

Parent Component ------------------------------------------------------------------> Child Compo
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
