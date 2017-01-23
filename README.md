P : 36

Global packages for Angular2
- npm
$ npm install -g typescript
$ npm install -g angular-cli

Type script compiler to try it in the console:
$ npm install -g tsun
$ tsun


Creating new project:
$ ng new angular2_hello_world

Start application
$ ng serve

Generate component
$ ng generate component hello-world
$ ng generate component user-item
$ ng generate component user-list

Remove folder with long name:
  $ npm install -g rimraf
  $ rimraf <dir>

VScode Hot Keys:
  "Shift + Alt + F" = auto formatting
 
insert to settings.json 
  "editor.formatOnSave": true
 

// loop in html
1 <ul>
2   <li *ngFor="let name of names">Hello {{ name }}</li>
3 </ul>

// use a component
1 <ul>
2   <app-user-item *ngFor="let name of names">
4   </app-user-item>
5 </ul>


// button implementation
1  <form class="ui large form segment">
2    <h3 class="ui header">Add a Link</h3>
3    
4    <div class="field">
5      <label for="title">Title:</label>
6      <input name="title" #newtitle> <!-- changed -->
7    </div>
8    <div class="field">
9      <label for="link">Link:</label>
10     <input name="link" #newlink> <!-- changed -->
11   </div>
12
13   <!-- added this button -->
14   <button (click)="addArticle(newtitle, newlink)"
15       class="ui positive right floated button">
16     Submit link
17   </button>
18
19 </form>

8  export class AppComponent {
9    addArticle(title: HTMLInputElement, link: HTMLInputElement): boolean {
10     console.log(`Adding article title: ${title.value} and link: ${link.value}`);
11     return false;
12   }
13 }


// Event Emitter example:
1   @Component({
2     selector: 'single-component',
3     outputs: ['putRingOnIt'],
4     template: `
5       <button (click)="liked()">Like it?</button>
6     `
7   })
8 
9   class SingleComponent {
10    putRingOnIt: EventEmitter<string>;
11  
12    constructor() {
13      this.putRingOnIt = new EventEmitter();
14    }
15  
16    liked(): void {
17      this.putRingOnIt.emit("oh oh oh");
18    }
19  }
// use it in the parent component
1   @Component({
2     selector: 'club',
3     template: `
4       <div>
5         <single-component
6           (putRingOnIt)="ringWasPlaced($event)"
7           ></single-component>
8       </div>
9     `
10  })
11  class ClubComponent {
12    ringWasPlaced(message: string) {
13      console.log(`Put your hands up: ${message}`);
14    }
15  }
16  
17  // logged -> "Put your hands up: oh oh oh"


// attach class type "item" to the component
84  @Component({
85    selector: 'product-row',
86    inputs: ['product'],
87    host: {'class': 'item'},
88    template: `
89    ...


// displaying an image
81 @Component({
82   selector: 'product-image',
83   host: { class: 'ui small image' },
84   inputs: ['product'],
85   template: `
86      <img class="product-image" [src]="product.imageUrl">
87   `
88 })


// breadcrumbs for example "Women > Apparel > Jackets & Vests"
50  @Component({
51    selector: 'product-department',
52    inputs: ['product'],
53    template: `
54      <div class="product-department">
55        <span *ngFor="let name of product.department; let i=index">
56          <a href="#">{{ name }}</a>
57          <span>{{i < (product.department.length-1) ? '>' : ''}}</span>
58        </span>
59      </div>
60    `
61  })
62  class ProductDepartment {
63    product: Product;
64  }



/*
 * Built-in Components
 */
 
1 <div *ngIf="false"></div> <!-- never displayed -->
2 <div *ngIf="a > b"></div> <!-- displayed if a is more than b -->
3 <div *ngIf="str == 'yes'"></div> <!-- displayed if str holds the string "yes" -\
4 ->
5 <div *ngIf="myFunc()"></div> <!-- displayed if myFunc returns a true value \
6 -->


1 <div class="container" [ngSwitch]="myVar">
2   <div *ngSwitchCase="'A'">Var is A</div>
3   <div *ngSwitchCase="'B'">Var is B</div>
4   <div *ngSwitchDefault>Var is something else</div>
5 </div>


// add custom dynamic styling
// example 1
12 <div [style.background-color]="'yellow'">
13  Uses fixed yellow background
14 </div>

20 <div [ngStyle]="{color: 'white', 'background-color': 'blue'}">
21  Uses fixed white text on blue background
22 </div>

// example 2
11 .bordered {
12   border: 1px dashed black;
13   background-color: #eee;
14 }

9  <div [ngClass]="{bordered: false}">This is never bordered</div>
10 <div [ngClass]="{bordered: true}">This is always bordered</div>


// ng loop, getting indexes
60 <div class="ui list" *ngFor="let c of cities; let num = index">
61   <div class="item">{{ num+1 }} - {{ c }}</div>
62 </div>

// none compilable html part
7   template: `
8     <div class='ngNonBindableDemo'>
9       <span class="bordered">{{ content }}</span>
10      <span class="pre" ngNonBindable>
11        &larr; This is what {{ content }} rendered
12      </span>
13    </div>
14  `




/*
 * Forms in Angular 2
 */
 
 
1   import { Component } from '@angular/core';
2   
3   @Component({
4     selector: 'demo-form-sku',
5   
6   template: `
7     <div class="ui raised segment">
8       <h2 class="ui header">Demo Form: Sku</h2>
9       <form #f="ngForm"
10         ngSubmit)="onSubmit(f.value)"
11         class="ui form">
12 
13        <div class="field">
14          <label for="skuInput">SKU</label>
15          <input type="text"
16            id="skuInput"
17            placeholder="SKU"
18            name="sku" ngModel>
19        </div>
20 
21        <button type="submit" class="ui button">Submit</button>
22      </form>
23    </div>
24  `
25  })
26  export class DemoFormSku {
27    onSubmit(form: any): void {
28      console.log('you submitted value:', form);
29    }
30  }


/*
 * Reactive Forms with FormBuilder
 */
 
1 import { Component } from '@angular/core';
2 import {
3   FormBuilder,
4   FormGroup
5 } from '@angular/forms';
6
7 @Component({
8   selector: 'demo-form-sku-builder',


29  export class DemoFormSkuBuilder {
30    myForm: FormGroup;
31  
32    constructor(fb: FormBuilder) {
33      this.myForm = fb.group({
34        'sku': ['ABC123']
35      });
36    }
37  
38    onSubmit(value: string): void {
39      console.log('you submitted value: ', value);
40    }
41  }


/*
 * Adding Validations
 */
 
function skuValidator(control: FormControl): { [s: string]: boolean } {
  if (!control.value.match(/^123/)) {
    return {invalidSku: true};
  }
}

export class DemoFormWithCustomValidations {
  myForm: FormGroup;
  sku: AbstractControl;

  constructor(fb: FormBuilder) {
    this.myForm = fb.group({
      'sku':  ['', Validators.compose([
        Validators.required, skuValidator])]
    });

    this.sku = this.myForm.controls['sku'];
  }

  onSubmit(value: string): void {
    console.log('you submitted value: ', value);
  }
}

/*
 * Form message, field coloring
 */
19 <div class="field"
20 [class.error]="!sku.valid && sku.touched">

26 <div *ngIf="!sku.valid"
27 class="ui error message">SKU is invalid</div>
28 <div *ngIf="sku.hasError('required')"
29 class="ui error message">SKU is required</div>



 