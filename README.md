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
    <div> {{ message }}</div>
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
        name: 'iPhone XR',
        price: 789,
        color: 'Black',
        discount: 5.5
    };

    getDiscountedPrice(): number {
        return this.product.price - (this.product.price * this.product.discount / 100);
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
        name: 'iPhone XR',
        price: 789,
        color: 'Black',
        discount: 5.5,
        pImage: '/assets/images/iphoneXR.png'
    };

    getDiscountedPrice(): number {
        return this.product.price - (this.product.price * this.product.discount / 100);
    }
}

```

```html

<!-- we can do it with string interpolation like this -->
<!-- <img src = {{product.pImage}}> -->

<!-- we can do it with property binding like this
Inside "" you can write any typescript expression -->
<img [src] = "product.pImage">
<img bind-src = "product.pImage">     <!-- Another syntax for property binding -->

<p>Product name: {{product.name}}</p>
<p>Original price: {{product.price}}</p>
<p>Product color: {{product.color}}</p>
<p>Discounted price: {{getDiscountedPrice()}}</p>

```


Note: If we can do same stuff with string interpolation why do we need property binding?
Ans: For html attributes like hidden, Disabled, Checked we need to use property binding only there string interpolation won't work.

- Type 2: Two Way Data Binding
Component to View View to Component

Two-way data binding binds data from component class to view template and view template to component class. It is a combination of property binding & event binding.
