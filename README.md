selector -> componanent named define which is used in access componanent in other componanent
imports -> import the other side to componanent like header,footer [header,footer or any directives]
templateurl -> seprate the html file attach
styleurl -> seprate css file attacg
template -> in html write on this ts file

signal -> take value set or update in componanent
user interact handle by (click,mouseover,mouseleaver etc....)


define the varible
variblename : datatype = value;


@input => from parent to child <- or -> Top To Bottom
Here Header is Child And And App Is Parent

(Parent)
export class App {
  Username: string = "Sujal Gopani";
}
//Given to Value to Child in html page
<app-header [Name]="Username"></app-header> // Take Parent Value from parent componanent
<main class="main">
  <p><strong>Message from Child:</strong> {{ parentmsag }}</p>
</main>
<app-footer></app-footer>


(Child)
import Input First 
export class Header {
@Input() Name: string =''; 
}
// apply Name In Html Page
<h1>UI For Header !</h1>
<p>Welcome {{Name}}</p>



@Output from child to parent <- Or -> botton To Top
from header to app

(child)
// first import output
export class Header {
//output
@Output() Sendmsg = new EventEmitter<string>();

sendmessage(){
  this.Sendmsg.emit("Message Send Sucessfully from Header !");
}
}

// app.html
<app-header (Sendmsg)="receivedfromchild($event)"></app-header>
<main class="main">
  <p><strong>Message from Child:</strong>{{parval}}</p>
</main>
<app-footer></app-footer>



@placeholder → before loading starts
@loading → while loading in progress
@defer → after loading completed
@loading(minimum 2s) -> take 2 second loading
@defer(on viewport) -> When Content Is scrolling and see the on screen then it's load


ngOptimizedImage for lazy loading for browser which work for improve the browser work
	ngsrc
	priority
	
	
	
Routing : pages change without the browser reloading
Work In app.routes.ts File
import { Routes } from '@angular/router';
import { TestRoutes } from './test-routes/test-routes';
import { App } from './app';


export const routes: Routes = [
  {path:'',component:App,title: 'App Home Page',}, // here bedefault root is app component 
  {path:'test',component:TestRoutes} // when user hit the requesrt the test then show test component
];

EX :->
<app-header></app-header>

<router-outlet></router-outlet>

<app-footer></app-footer>
🎯 Result:
✅ / → shows HomeComponent
✅ /test → shows TestRoutes
✅ App component ek j vaar load thay (as the container).



routlink : 

// import first in ts file
  imports: [Header,Footer,RouterOutlet,NgOptimizedImage,RouterLink],

// after used in the html file
<nav>
  <a routerLink="">🏠 Home</a> | // default request like '/'
  <a routerLink="test">🧪 Test</a> // /test request to serve the test component
</nav>



Form Overview 
  import FormsModule to use the angular form servies in the simple html page
 
   imports: [FormsModule]
   // typescript method to handle data which is occur from form
  onSubmit(data: any) {
    console.log("Form Submitted:", data);
  }
  
  
<h2>Template Driven Form using FormsModule</h2>

<form #userForm="ngForm" (ngSubmit)="onSubmit(userForm.value)">
  <label>Name:</label>
  <input type="text" name="name" ngModel>

  <label>Email:</label>
  <input type="email" name="email" ngModel>

  <button type="submit">Submit</button>
</form>

// getting value live from html elemtn by [(ngModel)]

app.ts
templatevalue:string  = '';

// app.html
<h3>{{templatevalue}}</h3>

<input type="text" id="fname" [(ngModel)]="templatevalue"/>
when user write the something in the text box then automaticaly live see in the h3 tag because of the [(ngModel)]


reactive form : 
// first of the inport like that
import { ReactiveFormsModule } from '@angular/forms';
  
10/11/2025
-----------  
  
Signal : used like a store or set or update value

.ts file 
 count = signal(0);
  addsignal(){
    this.count.set(this.count()+1);
  };

  subsignal(){
    this.count.update(x=> x -1);
  };
 
.html file
 <p><button (click)="subsignal()">-</button> {{count()}} <button (mousedown)="addsignal()">+</button></p>

 ◘ Effect : 
	- used like a react useeffect when the signal is changed then signal re check in the angular app.
	- Must Be write in the constructor.
	ex.
	 constructor() {
    // one time console this effect
    effect(() => {
      console.log(`The count `);
    });
	}
	 constructor() {
    // then the entered count signal changed then effect call
    effect(() => {
      console.log(`The count is: ${this.count()}`);
    });
	}
	
	◘ Effect Destroy :
		private destroyFn!: EffectRef; 
			// makeing
		  constructor() {
			this.destroyFn = effect(() => {
			  console.log("Data IS : " + this.count());
			});
		  }
			// destroye
		  destroyeffect(){
			this.destroyFn.destroy();
			console.log("Signal Is Cleaned !");
		  }
		  
◘ LinkSignal : When the one signal is dependent with other one signal then the result if the one signal is changed then automatically other one is changed so that not happen in the application angular provide the linksignal.
	- ex.
		.ts file
			  opt = signal(["sujal","Henil","Kush"]);
			  selectedopt = linkedSignal(()=>this.opt()[0]);
				changeToKush() {
				this.selectedopt.set(this.opt()[2]);
			  }
			  seeval(){
				console.log(this.selectedopt())
			  }
			  updateOptions() {
				this.opt.set(["Meet", "Ravi", "Jigar"]);
			  }
		.html file
			<button (click)="changeToKush()">Change to Kush</button>
			<button (click)="updateOptions()">Update Options</button>
			<button (click)="seeval()">See Console Value</button>

◘ Resource : 
		- In the Angular for async (promise-based) Api Call in the angular application
		- when the we used the signal that time data is the synchronous but in the 
		- real world angular application has the asynchronous data is used which is from take server/api.
		- that type of data is fetched by asynchronous style.
		
		Ex. 
			.ts file :
			  userId = signal('1');

			  userRes = resource({
				params: () => ({ id: this.userId() }),
				loader: async ({ params }) => {
				  const res = await fetch(`https://jsonplaceholder.typicode.com/users/${params.id}`);
				  return res.json();
				}
			  });

			  reload() {
				this.userRes.reload();
			  }
  
			.html file :
				<h3>User Details</h3>
				@if (userRes.isLoading()) {
				  <p>Loading user...</p>
				} @else if (userRes.error()) {
				  <p>Error: {{ userRes.error() }}</p>
				} @else if (userRes.hasValue()) {
				  <p>Name: {{ userRes.value().name }}</p>
				}
				<button (click)="reload()">Reload</button>

--------------------------------------
11/11/2025
-------

○ Component

	Every component must have:

		◘ A TypeScript class with behaviors such as handling user input and fetching data from a server
		◘ An HTML template that controls what renders into the DOM
		◘ A CSS selector that defines how the component is used in HTML

	 ->You provide Angular-specific information for a component by adding a @Component
		Ex. 
		@Component({
		  selector: 'profile-photo', // componanent name
		  template: `<img src="profile-photo.jpg" alt="Your profile photo">`,// in component html
		  styles: `img { border-radius: 50%; }`, // in componanent css
		  templateUrl: 'profile-photo.html', // septrate html
		  styleUrl: 'profile-photo.css',	// seprate css
		})
		export class ProfilePhoto { }
		
		-> Using components by Imports Selector Name
		Ex.
		import {ProfilePhoto} from './profile-photo';
		@Component({
		  // Import the `ProfilePhoto` component in
		  // order to use it in this component's template.
		  imports: [ProfilePhoto],
		  /* ... */
		})
		export class UserProfile { }
		
	○ Selector
		- selector is the name of the angular componanent which is help to call and renders in the anywhere for rendering.
		EX.
			@Component({
			  selector: 'profile-photo',// this the profile is the name used for call and render 
			  ...
			})
			// Call the profile-photo componanent in the other componanent
			@Component({
			  template: `
				<profile-photo />
				<button>Upload a new profile photo</button>`,
			  ...,
			})
			
			Types of selectors
			Angular supports a limited subset of basic CSS selector types in component selectors:

			♥ Selector type	Description	Examples
				♦ Type selector	Matches elements based on their HTML tag name, or node name.	profile-photo
				♦ Attribute selector	Matches elements based on the presence of an HTML attribute and, optionally, an exact value for that attribute.	[dropzone] [type="reset"]
				♦ Class selector	Matches elements based on the presence of a CSS class.	.menu-item
				
		◘ The :not pseudo-class :-
			Ex.
				<p>Hello</p>
				<p class="skip">Hi</p>
				<p>Welcome</p>
				
				p:not(.skip) {
				  color: red;
				} // here meaning of this, apply red color where skip class not 	written 
			Other: 
				div:not(.box, .card) {
				  border: 1px solid black;
				}
				
				<ul>
				  <li>Apple</li>
				  <li class="special">Mango</li>
				  <li>Banana</li>
				</ul>
				css
					
					li:not(.special) {
					  color: green;
					}
					
		◘ attribute selector :
			selector: 'button[app-save]', // like this
			<button app-save (click)="onSave()">Save File</button> // used like this
			
	○ Styling components : 
		◘ Scopping Stylling 
			-> This is the define the css like set the limit of the css
			-> By encapsulation : ViewEncapsulation.[Emulated (default)(apply css on single componanent),ShadowDom,None(apply global css on all componanent)]
			
	○ Accepting data with input properties :
		◘ input mainlly used for a take the value and display in the same componanent.
			Ex. 
			header.ts file :
				value = input(7);
			header.html file :
				<h3>{{value()}}</h3>
				
			app.ts
				first import test
			app.html
				<app-header [value]="211"></app-header> // asign the value of value.
			
		◘ Reading inputs: 
			// Declare an input named 'value' with a default value of zero.
			value = input(0);
			
			// Create a computed expression that reads the value input
			label = computed(() => `The slider's value is ${this.value()}`);
			call label in the html file
		
		◘ Required inputs:
			  value = input.required<number>();
			  
		◘ Input transforms:
			-> when we want the data is look like a specifit format but user enter not satifield format then used the Input transforms.
			Ex.
				test.ts file : 
					label = input('', {transform: trimString});// calling function
					// maker the one format function 
					function trimString(value: string | undefined): string {
						return value?.trim() ?? '';
					}
				app.html:
					<test-slider [label]="systemVolume" />
					// call the test in the app and given the value from the parent app componanent side.
		
		◘ Type Checking :
			-> input type cheking by tranform 
			Ex.
			@Component({/*...*/})
			export class CustomSlider {
			  widthPx = input('', { transform: appendPx });
			}

			function appendPx(value: number): string {
			  return `${value}px`;
			}// user enter number but function return string
		
		◘ Alias In Input :
			- when we want the calling the parent level to child or child to parent level but not a declared named calling the deferent name in the other componant then used the Alias concept in input.
			Ex.
			@Component({/*...*/})
			export class CustomSlider {
			  value = input(0, { alias: 'sliderValue' });
			}
			// calling componanent in other componanent
			<custom-slider [sliderValue]="50"></custom-slider>
			// here make a value named input but calling the sliderValue by helping alias properties.
			
		◘ Model Input : 
			- in the simple input method when we used the input then parent to child flow is gone or data flow is going on the parent to chilld one way.
			- but the model input is used for a two data binding like a data share between parent to child or child to parent.
			Ex.
				(child)
				Header.ts
					 label =model(0);
					  incre(){
						this.label.update(x=> x+1);
					  }
				header.Html
					<h2>{{label()}}</h2>
					<button (click)="incre()">+</button>
				
				(Parent)
				app.ts
					export class App {
					  testingval  = signal(1);
					}
				App.Html
					<p>Parent Value : {{testingval()}}</p>
					<app-header [(label)]="testingval"></app-header>
				// here pass the parent value but increment from child and reflect in the parent componanent also.

		◘ Implicit change events : 
				- when the we used the model input then angular makes automatic change method.
				- when the chid is changed then refclect change method in the parent.
		
		◘ Used @Input :
			- it's a same as a simple input used only deferent sytax at the end of the meaning is same as a simple input.
			Ex.
			 @Input() value = 0;
			 @Input({required: true}) value = 0;
			 @Input({transform: trimString}) label = '';
			 @Input({alias: 'sliderValue'}) value = 0;
			 
	○ Inputs with getters and setters :
		- getter and setters are used for apply login in between value get and during set.
		Ex. (simple getter and setters)
		test.ts file :
			private label:string="";

			set newlabel(value : string){
				console.log("value Set...");
				this.label = value;
			}
			  
			get newlabel():string{
				return this.label.trim();
			}  	
			
		test.html :
			<input type="text" placeholder="Enter Name" [(ngModel)]="newlabel">
			<p>Name Is : {{newlabel}}</p>
		// in this code when the want to get value in the <p> tag then getting value with trim() login so, when the user enter data in the input then trim work and clean all unnessary spaces.
		
		// Inputs with getters and setters :
			header.ts (child)
				private _name: string = '';
				@Input()
				  set name(value: string) {
					console.log('Input value set:', value);
					this._name = value.toUpperCase(); // logic: uppercase માં convert
				  }

				  get name(): string {
					return this._name;
				  }
			header.html
				<h3>User Name: {{ name }}</h3>
			
			app.ts (parent)
				parentName = 'Sujal';
			
			app.html
				<input type="text" [(ngModel)]="parentName" placeholder="Enter Name">

				<app-header [name]="parentName"></app-header>
					
------------------
12/11/2025
----------
○ Custom events with outputs
	- output is the used for a access or pass value form child to parent.
	Ex.
		header.ts (child)
			childvalueaccept = output<string>();
			sendvalue(){
				console.log("Send message...");
				this.childvalueaccept.emit("From child !");
			}

		header.html
			<p>Child Compo : </p>
			<button (click)="sendvalue()">Send Parent</button>

		app.ts (parent)
			parval:string = "Waiting Message!";
			acceptvalue(val : string){
				this.parval= val;
			}  
  
		app.html
			<h1>Parent Comp : </h1>
			<p>Child Message : {{parval}}</p>
			<app-header (childvalueaccept)="acceptvalue($event)"></app-header>

	○ Customizing output names:
		@Component({/*...*/})
			export class CustomSlider {
			  changed = output({alias: 'valueChanged'});
			}
			
	○ Content projection with ng-content :
		- ng-content is the used for a add the html from parent to child calling.
		- in short when we call child componanent in the parent then we can't add the parent html in the child componanent calling side but ng-content is help a adding html from parent to child.
		
		Ex.
		child.html
			<div style="border:1px solid gray; padding:10px; border-radius:8px;">
			  <h3>💳 Custom Card</h3>
			  <ng-content></ng-content>
			  <p style="color:gray;">-- Card Footer --</p>
			</div>
		parent.html
			<h2>Parent Component</h2>
			<app-header>
			  <p>This is the content from parent component!</p>
			  <button>Click Me!</button>
			</app-header> // here add some html from parent side.
		
	○ Multiple content placeholders: 
		-in the single page multiple ng-content used.
		Ex.
			Child 
			 <div style="border:1px solid gray; border-radius:8px; padding:10px;">
			  <header style="font-weight:bold; color:blue;">
				<ng-content select="[header]"></ng-content>
			  </header>

			  <section>
				<ng-content></ng-content>
			  </section>

			  <footer style="font-style:italic; color:gray;">
				<ng-content select="[footer]"></ng-content>
			  </footer>
			</div>
					
			Parent
			<h2>Parent Component</h2>
			<app-header>
			  <div header>✨ This is Header ✨</div>
			  <p>This is main body content projected from parent.</p>
			  <div footer>📞 This is Footer info</div>
			</app-header>
			<router-outlet />

	○ Host elements :
		- in the @componanent side Host are availble.
		- main work of the host is handle current componanent html,style or it's frontend related login like click,over,leave etc...
		
		Ex.
			ts.file
				@Component({
				  selector: imports: templateUrl: styleUrl: ,
				  host:{
				   '[style.border]': "'2px solid black'",
					'(mouseover)': 'onClick()'
				  }
				})


				export class Header {
				  test :string= "sujal"
				  onClick() {
					const te= "sujal gopani";
					this.test = te;
				  }
				}
			
			html.file
				<p>User name: {{test}}</p>
			// here apply border with black color and when  mouse over in this component then convet ot transform sujal to sujalgopani.
			// so host are handle current ts.file or it's html file by hover or etc applys.
			
			
-------------------
13/11/2025
----------

	○ Life Cycle : 
		1.ngOnInit-> It's call when the all componanent are inttialize succesfully on angular application.
		
		2.ngOnChanges -> It's call When the @Input Are change from parent value to child componanent. also it's has one object named 'SimpleChanges' that's provide previews, current, or first time it's changed or not example are Below.
		
		Ex.
			Header.ts(child)
			@Input() name: string = '';

			  ngOnChanges(changes: SimpleChanges) {
				for (const prop in changes) {
				  const change = changes[prop];
				  console.log(`Previous: ${change.previousValue}`);
				  console.log(`Current: ${change.currentValue}`);
				  console.log(`Is first change: ${change.firstChange}`);
				}
			  }			
			
			App.ts(Parent)
				username = 'Sujal';
				  				  
			App.html
				<h2>Parent Component</h2>
				<input [(ngModel)]="username" placeholder="Enter name">
				<app-header [name]="username"></app-header>
		
		3.ngOnDestroy : When The Component Is Destroy Then ngOnDestroy Is Call.
			|     Concept     |   Description                                    |
			| --------------- | ----------------------------------------------------- |
			| `ngOnDestroy()` | Traditional lifecycle hook for cleanup                |
			| `DestroyRef`    | Modern injectable object for cleanup callbacks        |
			| `.onDestroy()`  | Method to register cleanup logic                      |
			| `.destroyed`    | Property to check destruction status                  |
			| Benefit         | Cleaner, reusable, and function-based cleanup pattern |
			
			◘ngOnDestroy(): EX
				
			  timerId: any;

			  constructor() {
				this.timerId = setInterval(() => {
				  console.log('⏳ Timer running...');
				}, 1000);
			  }

			  ngOnDestroy() {
				console.log('🧹 Component Destroy Thayi Gayo, cleanup time!');
				clearInterval(this.timerId);
			  }
			
			◘DestroyRef(injectable) : EX
				 private destroyRef = inject(DestroyRef);

				  constructor() {
					console.log('✅ UserProfile Loaded');

					// Register cleanup logic when component is destroyed
					this.destroyRef.onDestroy(() => {
					  console.log('❌ UserProfile Destroy Thayi Gayo!');
					});
				  }
			
			◘.destroyed(check status) : EX
				 destroyRef = inject(DestroyRef);

				  constructor() {
					setTimeout(() => {
					  if (!this.destroyRef.destroyed) {
						console.log('✅ Safe to update UI');
					  } else {
						console.log('🚫 Component already destroyed!');
					  }
					}, 5000);
				  }
			  
		○ ngDoCheck : It's Every Time Runs When The Angular Change Detection Happen  Or It's Runs When The Angular componanent Data checked.
		- When The One Field Or One Variable Or Change Anything In The Component Are update Then this Runs.
			Ex.
				name : string = "Test";
				ngDoCheck(){
					console.log(this.name.toUpperCase());
				}
				
				<input type="text" placeholder="Write Anything" [(ngModel)]="name"/>
				<p> Value Is :{{name}}</p>
			// here when the name properties are change then ngDoCheck Runs Every Time.
		
		○ ngAfterContentInit :
			- When the ng-content are fully inttialize Then after ngAfterContentInit runsa.
			
			Ex.
			 ngAfterContentInit() {
				console.log('✅ ngAfterContentInit called - Projected content initialized!');
			  }
			
			<h1>Exaple For ngAfterContentInit</h1>
			<ng-content></ng-content>
			// When the ng-content is update or fill the record in this ng-content then ngAfterContentInit is calls.
			
		
		○ ngAfterContentChecked : 
		 - When The ng-content has data but when user update this data then ngAfterContentChecked runs.
	
	○ Referencing component children with queries : 
		- Developers most commonly use queries to retrieve references to child components, directives, DOM elements, and more.
		Two Type Of queries :
			1-> View Query : it's used for a make references from a child elements by @ViewChild or viewChild().
			2-> Content queries : Content queries retrieve results from the elements in the component's content from ng-content.
			
			Ex.
				child :
					@Component({
					  selector: 'child-box',
					  template: `<p>I'm a child!</p>`
					})
					export class ChildBox {}
				Parent : 
					@Component({
					  selector: 'app-root',
					  template: `
						<h2>Parent Component</h2>
						<child-box></child-box>
					  `
					})
					export class AppComponent implements AfterViewInit {
					  // View Query to access Child Component
					  child = viewChild(ChildBox);

					  ngAfterViewInit() {
						console.log("Child component:", this.child());
					  }
					}// here when the child componanent present or if the child-box componanent is available in the parent componanent then the view query run thase.
				
			◘ Content queries :
			 - access the child value to parent or parentto child by it's reference.
				Ex.
					child.ts
						export class Child {
						  tittle = 'Hellow Greeting From Child Compo !';
						}
						
					parent.ts
						@Component({
						  selector: 'app-parent',
						  imports: [],
						  template: `
						  <p>Parent Compo :</p>
						  <p>{{childval()}}</p>
						`,
						  styleUrl: './parent.css',
						})

						export class Parent {
						  chudlref = contentChild(Child);
						  childval = computed(()=> this.chudlref()?.tittle);
						}
						
					App.Html
					<app-parent>
					  <app-child></app-child> 
					</app-parent> 
						
						
----------------
14/11/2025
----------
	○ Required queries : 
		- It's a simple like query But main thing is, Developer Can Compulsoury Give a Child Element, If The Not Given Then Angular Is Throw Error.
		Ex.
			chudlref = contentChild.required(Child);
			// here used the required For Apply This Concept.
			
	○ Query locators : 
		- In the viewChild, contentChild, viewChildren, contentChildren First Parameter Is locators. It's Return The String Location OR String Content of the Html elements.
		- It's Return Type is ElementRef<HTMLButtonElement>.
		- In short It's Return the Content Which is Fill In the html elements or tag.
		
		Ex.
		@Component({
		  selector: 'app-parent',
		  imports: [],
		  template: `
		  <p #nameprop>Sujal Sujal Sujal Angular</p>
		`,
		  styleUrl: './parent.css',
		})

		export class Parent {
		  nameval = viewChild<ElementRef<HTMLParagraphElement>>('nameprop');

		  ngAfterViewInit(){
			console.log(this.nameval()?.nativeElement.textContent);
		  }
		}// here console is <p> tag content.
	
	○ Queries and the injector tree : 
		- When we want to use viewchild or contentChild that time we access that's reference on their line by line like a parent->chid->grandchild like this.
		- but here injector is used for a direct access the component by it's token value and access the direct value.
		Ex.
			Child.ts
				import { Component, InjectionToken } from '@angular/core';
			
			// here make a tokem
			export const ITEM_TOKEN = new InjectionToken<string>('item-token');

			@Component({
			  selector: 'app-child',
			  imports: [],
			  styleUrl: './child.css',
			  // provide token name and it's value.
			  providers: [
				{ provide: ITEM_TOKEN, useValue: 'Hello from Provider!' }
			  ],
			  template: `<p>Child Component Loaded...</p>`
			})
			
			parent.ts
				@Component({
				  selector: 'app-parent',
				  imports: [Child],
				   template: `
				   <h2>Parent Component</h2>
				   <app-child></app-child>
				`,
				  styleUrl: './parent.css',
				})

				export class Parent {
				  item = viewChild(ITEM_TOKEN);

				  ngAfterContentInit() {
					console.log('Child Provider Value:', this.item());
				  }
				}// viewchild direct token name and access the token value.

	
	○ ViewChild,ElementRef,viewChildren Meaning :
		- if the we can access the other element in the current element then we should use the viewchid.
		- if we can access the cureent element attribute when use the elementref by using with #.
		- if the multimple element import in the current element when we should use the viewChildren with querylist.
		

○ Using DOM APIs :
	- In the JS We Use The DOM For Document Access Or Changing By Using DOM, That Same Used In The Angular Used The DOM APIs.
	- Access The Document By ElementRef.
	Ex.
		@Component({...})
		export class ProfilePhoto {
		  constructor() {
			const elementRef = inject(ElementRef);
			console.log(elementRef.nativeElement);
		  }
		}// that's return Full Element In The Browser.
	
		◘ afterEveryRender  :
			- Render Every Render  
			
		◘ afterNextRender
			- Render Next Render Or Render One Time
			
		Ex.
			@Component({...})
			export class ProfilePhoto {
			  constructor() {
				const elementRef = inject(ElementRef);
				afterEveryRender & afterNextRender (() => {
				  // Focus the first input element in this component.
				  elementRef.nativeElement.querySelector('input')?.focus();
				});
			  }
			}
	
	○ Inheritance :
		- when we perform or access the child function value or etc from child when we apply the Inheritance concept by using extends.
		
		Ex.
			child.ts
				@Component({
				  selector: 'app-child',
				  imports: [],
				  styleUrl: './child.css',
				  template: ``
				})

				export class Child {
				  name = "Sujal";
				  walk() {
					console.log("Person can walk");
				  }
				}
			
			Parent.ts
				@Component({
				  selector: 'app-parent',
				  imports: [],
				   template: ``,
				  styleUrl: './parent.css',
				})

				export class Parent extends Child {
				  study() {
					console.log("Student is studying");
				  }
				  
				  ngOnInit() {
					this.walk();
					this.study();
					console.log(this.name);
				  }
				} // using extends apply Inheritance concept.
				
	○ NgComponentOutlet : 
		- when we perform a condition bases componanent rendering that time NgComponentOutlet is used.
		-Ex. If the user can login in the website but developer make one login page for regular user or main admin so that time login user role based rending is required so NgComponentOutlet is used.
		
		Ex.(simple condition based)
		import { NgComponentOutlet } from '@angular/common';
		@Component({
			selector: 'app-root',
			imports: [RouterOutlet,NgComponentOutlet],
			templateUrl: './app.html',
			styleUrl: './app.css'
		  })


		  export class App {
			user = true;
			getBioComponent(){
			  return this.user? Parent : ChildComponent;
			}
		  }
		 
		html
			<ng-container *ngComponentOutlet="getBioComponent()" /> `
			<router-outlet />// in this ng-container fill the componanent by throw out the ngComponentOutlet.
			// here if the user is true then parent is call other side child is called or render in the main app.ts
			
			
○ ViewContainerRef : 
	- At the Runtime When We Want to Display Dynamic UI Then Used ViewContainerRef.
	- In short Dynamic Display UI At a Runtime.
	
	Ex.
		Parent.ts
			- in this file contain only html code like '<h2>Parent Component Loaded</h2>
              <h3>Parent Here For Rendering By Content Ref</h3>'.
	 
		app.ts	
			export class App {
			   private views = inject(ViewContainerRef);
			   btn:boolean = false;
			   loadparent(){
				this.views.createComponent(Parent);
				this.btn = true;
			   }
			  }
		app.html 
			<button (click)="loadparent()" [disabled]="btn">Load Now Parent</button>
			// here when the user click the button then parent componanent are loaded in the app.ts at run time by using ViewContainerRef.
			
	○ Lazy-loading components :
		- When User Want to Show The Particular Component By Clicking Button or etc Then Used the lazy loading technique.
		- In this componanent is not loaded on the runtime but when click button or anything else happen that time componanent are rendered or import.
		- two type of lazy loading - @defer or ngComponentOutlet
		
		Ex.
			parent.ts
				- it's contain simple html text.
			
			app.ts
				- export class App {
					parentval : any | null = null;
					async loadpar(){
					  const {Parent} = await import('./parent/parent')
					  this.parentval = Parent;
					}
				  }
			app.html
				<h1>App Component</h1>

				@if(!parentval){
				  <button (click)="loadpar()">Login</button>
				}
				<ng-container *ngComponentOutlet="parentval"></ng-container>
				// here first of the parentval is null then out condition is !parentval (!null) means true so when the parentval is true that time button is show after clicking button that is vanish which means that is not show.
				// click tha button and dynamic import the parent in the app.ts so parentval has consume parent componanent so now it's not null so button is not show. (so that is lazy loading...)


○ Binding inputs, outputs and setting host directives at creation : 
	- when the use with simple template that time we use a @input or @output but when use the dynamic loading template by using ViewContainerRef that time inputbinding,outputbinding used.
	- in short when the used the simple way for pass data from parent to child that time used the simple method @Input(),@Outlet(),
		but when the used ViewContainerRef for dynamic componanent Rendering that time pass the value by using inputbinding,outputbinding which is latest v17+ angular method.
		
		Ex.
			child.ts
				@Component({
				  selector: 'app-child',
				  standalone: true,
				 
				  template: `
					 <h2>Child Component</h2>
					  <h4>Child Input From Parent : {{Childinput}}</h4>
					  <button (click)="Sendtoparent()">Send To PArent</button>
				  `
				})
				export class ChildComponent {
				 @Input() Childinput?: string ;
				 @Output() Childoutput = new EventEmitter<string>();

				 Sendtoparent(){
				  this.Childoutput.emit("I am Child How Are You Parent!");
				 }
				}
				
			parent.ts
				@Component({
				  selector: 'app-parent',
				  styleUrl: './parent.css',
				   template: `
				  <h2>Parent Component</h2>
				  <p>Parent Input From app : {{ParentInput}}</p>
				  <p>Parent Text Is : {{ReplyofParenttext}}</p>
				   <app-child
					 [Childinput]="Childinputvalue"
					  (Childoutput)="ReplyFromParent($event)"
				   ></app-child>
				  `
				,
				  imports: [ChildComponent]
				})

				export class Parent  {
				 Childinputvalue = "Hy Child, I am Parent";
				  ReplyofParenttext = "";

				  @Input() ParentInput?:string;
				  @Output() replyMessage = new EventEmitter<string>();


				  ReplyFromParent(msg : any | null){
					this.ReplyofParenttext = msg;
					this.replyMessage.emit(msg)
				  }
				}
				
			app.ts
				export class App {
					private vcr = inject(ViewContainerRef);
					Loadparent(){
					  import('./parent/parent').then(({Parent})=>{
						this.vcr.createComponent(Parent,{
						  bindings:[
							inputBinding('ParentInput',()=>'Hellow Parent I am App'),
							outputBinding('replyMessage',(val:any)=>{
							  console.log("Parent Output : ",val);
							})
						  ]
						})
					  })
					}
				  }
				  
			app.html
				<h1>App Component</h1>
				<button (click)="Loadparent()">Load Parent</button>
			// code Explain :
				- in the child component use @input(value consider from parent)
					@output(value give to parent)
				- in the parent use @input(value consider from the child)
					@output(value give from the app)
				- in the app.ts use a ViewContainerRef for using dynamically render parent by clicking button.
					in the app.ts we used the inputbinding,outputbinding which is above side.
					
○ ViewContainerRef.createComponent :
	- this is used when we want to make new component in the particular place.which means ViewContainerRef.createComponent is a make a new or create new component from given place in the current componanent.
		Ex.
		// child.ts
			@Component({
			  selector: 'app-child',
			  standalone: true,
			  template: `
				 <h2>Child Component</h2>
				 <p>Parent Message: {{ messageFromParent() }}</p>

			<button (click)="sendToParent.emit('Hello Parent!')">
			  Send To Parent
			</button>
			  `
			})
			export class ChildComponent {
			  messageFromParent = input<string>("");
			  sendToParent = output<string>();
			}
			
		parent.ts	
			@Component({
			  selector: 'app-parent',
			  styleUrl: './parent.css',
			   template: `
			 <h2>Parent Component</h2>
			<button (click)="loadChild()">Load Child Dynamically</button>
			<p>Message From Child: {{ childMsg() }}</p>
			<ng-container #box></ng-container>
			  `
			,
			  imports: []
			})

			export class Parent  {
			  vcr = inject(ViewContainerRef);
			  // signals
			  parentMsg = signal("Hello Child From Parent!");
			  childMsg = signal("");
			  loadChild() {
				this.vcr.createComponent(ChildComponent, {
				  bindings: [
					// INPUT → Parent → Child
					inputBinding("messageFromParent", this.parentMsg),
					// OUTPUT → Child → Parent
					outputBinding("sendToParent", (msg: string) => {
					  this.childMsg.set(msg);
					})
				  ]
				});
			  }
			}
	
		app.html
			
			<h1>App Component</h1>
			<app-parent></app-parent>
			// here make a simple example of the ViewContainerRef.createComponent.
			- in the chid.ts make a input and output for data pass from parent to child and child to parent.
			- in the parent given to string to child for input and retrive the string which is belong to child by output.
			- inputbinding is gives to data to child and outputbinding is read the data which is receive from child.
			- in short inputbinding is give and outputbinding is take.
	
								
------------------
25/11/2025
----------		
○ Advanced component :
	
	○ ChangeDetectionStrategy : 
		- this is a check the componanent when the anything happen in the componanent.
		- when the button clicked,settimeout,api responce,Keyborad key press when ChangeDetectionStrategy is check the componanent.
		- in this two mode is available : 1.Default, 2.OnPush
		- Default : Every time check componanent when the anything happen in the componanent.
		- OnPush : only check when we want not check every time, that check when the input change,componanent event handle(button,keybord),Promises run.
		- in short default is every time every componanent check. nut OnPush is check when we want.
		Ex.
			@Component({
			  selector: 'child',
			  template: '{{count}}',
			  changeDetection: ChangeDetectionStrategy.OnPush & Default
			})
			
	○ PreserveWhitespaces : 
		 - by default html not consider a extra space or tabs but PreserveWhitespaces is help to consider extra space or tabs in the code.
		 Ex.
			@Component({
			  selector: 'app-root',
			  templateUrl: './app.html',
			  styleUrl: './app.css',
			  preserveWhitespaces: true
			})
			// witout PreserveWhitespaces below code is show as a 'Hellow Sujal'
			<h1>App Component</h1>
			<p class="keep-space">
			  Hello
				Sujal
			</p>
			// but the use of PreserveWhitespaces is show as same as browser.
			
	○ Custom Element Schema :
		- bydefault angular not allow to out of html tag in the angular application.
		- but Custom Element Schema is help to write a custom tag or use a other framework tag in the angular. like a vue js tag is used in angular.
		- but main thing angular cannot a bind that tag.
		Ex.
			<my-web-component></my-web-component> // it's not a satisfied for angular.
			
			@Component({
			  selector: 'demo',
			  template: `
				<some-unknown-component></some-unknown-component>
			  `,
			  schemas: [CUSTOM_ELEMENTS_SCHEMA]
			})// use of this allow to write the custom componanent.
			
			✔ Allowed:
			<my-tag></my-tag>


			❌ But aa still angular not bind this :
			<my-tag [data]="value"></my-tag>  // because Angular check nahi kari shake

	○ custom elements:
	 - in the above point we know the angular is not accept the custom componanent but when we used schemas so we used the custom componanent.
	 - now create a custom element by helping createCustomElement() from @angular/element.
	 - so must install first npm install @angular/element.
	 Ex.
		1. Make componanent in the angular.
			import { Component, EventEmitter, Input, input, Output } from '@angular/core';
			import { NgComponentOutlet } from '@angular/common';

			@Component({
			  selector: 'app-child',
			  standalone: true,
			 
			  template: `
				 <h2>Child Component</h2>
			  `
			})
			export class ChildComponent {
			}
			// in this example we make two componanent parent or chid. this 2 componanent we can convert into custom element.
			
			2.make one file named RegiCustom (for registration of custom componanent)
				import { Component, Injector } from "@angular/core";
				import { Parent } from "../parent/parent";
				import { ChildComponent } from "../child/child";
				import { createCustomElement } from "@angular/elements";

				export function RegiCustom(injector : Injector){
				  const elements =[
					{name : 'parent-com',component:Parent},
					{name : 'child-com',component:ChildComponent},
				  ]

				  elements.forEach(e1=>{
					if(!customElements.get(e1.name)){
					  const e1class = createCustomElement(e1.component as any,{injector});
					  customElements.define(e1.name,e1class);
					  console.log("Rgistration success !");
					}
				  });
				}
				
			3.Register in main.ts : 
				import { bootstrapApplication } from '@angular/platform-browser';
				import { appConfig } from './app/app.config';
				import { App } from './app/app';
				import { RegiCustom } from './app/CustomEle/RegiCustom';

				bootstrapApplication(App, appConfig)
				.then(appref=> {
				  RegiCustom(appref.injector)
				})
				.catch((err) => console.error(err));
			
			4.use schemas : [CUSTOM_ELEMENTS_SCHEMA] for use a custom componanent in the angular.
				app.ts
				@Component({
					selector: 'app-root',
					templateUrl: './app.html',
					styleUrl: './app.css',
					schemas : [CUSTOM_ELEMENTS_SCHEMA]
					})

				  export class App {
				  }
			
			5.just write and used :
				app.html
				<h1>App Component</h1>

				<parent-com name="Sujal"></parent-com>
				<child-com></child-com>
				// named is must be write seprate by '-' like pare-com, childpar not allow.

○ Template : 
	- Template is Html file in the angular application.
	- componanent is a typescript or ts file of the angular or Template is the Html file of the Angular application.
	
	○ Binding dynamic text, properties and attributes :
		- 1) Text Interpolation — {{ }} // this is only for a text.
				<p>Your color preference is {{ theme }}.</p>
			Component:
				theme = 'dark';
		🔹 Output:
			Your color preference is dark.
		
		- 2) Interpolation with Signal : // when the signal is changed then UI re-render
				<p>Your color preference is {{ theme() }}</p>
			Component:
				theme = signal('dark');
				
		- 3) Property Binding [] :
			<button [disabled]="isFormValid()">Save</button>
			// used the disabled attribute in the angular UI
		
		- 4) Component / Directive Property Binding :
			Angular component
				<my-listbox [value]="mySelection()" />
			Directive :
				<img [ngSrc]="profilePhotoUrl()" />
		
		- 5) Attribute Binding — [attr.name]
			<ul [attr.role]="listRole()"></ul>
	
		- 6) Interpolation inside Attributes
			<img alt="Profile photo of {{ firstName() }}">
		
		- 7) CSS Class Binding
			<ul [class.expanded]="isExpanded()"></ul>
		
		- 8) CSS Style Binding — [style.property]
			<p [style.color]="theme()">Hello</p>

	
	○ Adding event listeners : 
		- Apply Events in the html attribute like keyup,keydown,click,mouseleave etc..
		
		○ Listening to native events :
			- When you want to add event listeners to an HTML element, you wrap the event with parentheses, (), which allows you to specify a listener statement.
			Ex.
				@Component({
				  template: `
					<input type="text" (keyup)="updateField()" />
				  `,
				  ...
				})
				export class AppComponent{
				  updateField(): void {
					console.log('Field is updated!');
				  }
				} //You can add listeners for any native events, such as: click, keydown, mouseover, etc.
		
		○ Accessing the event argument :
			- In every template event listener, Angular provides a variable named $event that contains a reference to the event object.
			- In short all the reference of the Html Attribute can catch in the ts file by the $event.
			Ex.
				@Component({
				  template: `
					<input type="text" (keyup)="updateField($event)" />
				  `,
				  ...
				})
				export class AppComponent {
				  updateField(event: KeyboardEvent): void {
					console.log(`The user pressed: ${event.key}`);
				  }
				}// here every time when the user enter text in the input attribute then console print the keybord presed key as a string, like if the user press the something like 'S' so console print the 'S' and so on.
				
		○ Using key modifiers :
			- When you want to capture specific keyboard events for a specific key, you might write some code like the following:
			- When We Want the Perform task When the user press specific key.
			Ex.
				@Component({
				  template: `
					<input type="text" (keyup)="updateField($event)" />
				  `,
				  ...
				})
				export class AppComponent {
				  updateField(event: KeyboardEvent): void {
					if (event.key === 'Enter') {
					  console.log('The user pressed enter in the text field.');
					}
				  }
				}
				
				@Component({
				  template: `
					<input type="text" (keyup.enter)="updateField($event)" />
				  `,
				  ...
				})
				export class AppComponent{
				  updateField(event: KeyboardEvent): void {
					console.log('The user pressed enter in the text field.');
				  }
				}// when the user press the enter key that time console is log.
				
				<!-- Matches shift and enter --> // press shift + Enter At a time
				<input type="text" (keyup.shift.enter)="updateField($event)" />

				<!-- Matches alt and left shift -->
				<input type="text" (keydown.code.alt.shiftleft)="updateField($event)" />
				// here when the press only alt + left shift when call method otherwise not.
				
		○ Preventing event default behavior :
			- When the use <a></a> and click on that time browser can reload, or anything else happen while clicking, but we not want that, so used a preventDefault() method which is belong to JS.
			Ex.
				@Component({
				  template: `
					<a href="#overlay" (click)="showOverlay($event)">REFRESH</a>
				  `,
				  ...
				})
				export class AppComponent{
				  showOverlay(event: PointerEvent): void {
					event.preventDefault();
					console.log('Show overlay without updating the URL!');
				  }
				}// wihtout preventDefault method when user click the <a> then browser redirect to #overlay direction but preventDefault method is not allow this, that help to not reload browser and stay there.
			
--------------
27/11/2025
----------
	○ Extend event handling :
		- In the Angular Application SomeTime Use a Click,Keyup,Mouse etc.. But When We Want to Make A Custom Event That Time It's Used.
		Ex.
			1. Make First File Which Declare All About Custom Event Defination By Extend EventManagerPlugin.(@Injectable,Constructor, Override Support or EventListner, Return Must be)
			
			LongP.ts
				import { Injectable, ListenerOptions } from "@angular/core";
				import { EventManagerPlugin } from "@angular/platform-browser";

				@Injectable()
				export class LongP extends EventManagerPlugin{
				  constructor(){
					super(document);
				  }

				  override supports(eventName: string): boolean {
					  return eventName == 'Longpress';
				  }

				  override addEventListener(element: HTMLElement, eventName: string, handler: Function, options?: ListenerOptions): Function {
					  let timeout : any;

					  const OnDown=()=>{
						timeout = setTimeout(() => handler(), 1000);
					  }
					  const OnUp=()=>{
						clearTimeout(timeout);
					  }

					  element.addEventListener('mousedown',OnDown);
					  element.addEventListener('mouseup',OnUp);

					  return()=>{
						element.removeEventListener('mousedown',OnDown);
						element.removeEventListener('mouseup',OnUp);
					  }

				  }
				}
				
				Main.ts
					bootstrapApplication(App,{
					  providers:[{
						provide : EVENT_MANAGER_PLUGINS,
						useClass : LongP,
						multi : true
					  }]
					});
				
				App.ts
					export class App {
						IsAlive = false;
						showMessage() {
						  this.IsAlive = true
						}
					  }
					  
				App.html
					<button [disabled]="IsAlive" (Longpress)="showMessage()">Dont Press Longer 1 second</button>
				// here we make a when the user press the button over 1 or more second then button is disabled automatic.
				
○ Two-Way Binding : 
	- Two-Way Binding is a What , Data is Pass From componanent to template or template to componanent, when the componanent value change then template value also automatic change as same aa apply in componanent or template.
	- [(ngmodel)] Is Used For Two Day Binding.
	
	Ex.
		1.Two-way binding with form controls:
			import { Component } from '@angular/core';
			import { FormsModule } from '@angular/forms';
			@Component({
			  imports: [FormsModule],
			  template: `
				<main>
				  <h2>Hello {{ firstName }}!</h2>
				  <input type="text" [(ngModel)]="firstName" />
				</main>
			  `
			})
			export class AppComponent {
			  firstName = 'Ada';
			}
			
		2.Two-way binding between components :
			child componanent:
				// './counter/counter.component.ts';
				import { Component, model } from '@angular/core';
				@Component({
				  selector: 'app-counter',
				  template: `
					<button (click)="updateCount(-1)">-</button>
					<span>{{ count() }}</span>
					<button (click)="updateCount(+1)">+</button>
				  `,
				})
				export class CounterComponent {
				  count = model<number>(0);
				  updateCount(amount: number): void {
					this.count.update(currentCount => currentCount + amount);
				  }
				} //  here model is the two data binding signal which is helping for updating child as well as parent.
				
			Parent componanent :
				// ./app.component.ts
				import { Component } from '@angular/core';
				import { CounterComponent } from './counter/counter.component';
				@Component({
				  selector: 'app-root',
				  imports: [CounterComponent],
				  template: `
					<main>
					  <h1>Counter: {{ initialCount }}</h1>
					  <app-counter [(count)]="initialCount"></app-counter>
					</main>
				  `,
				})
				export class AppComponent {
				  initialCount = 18;
				} // use the [()] for two way data binding.
				
○ Control flow :
		- Constrol flow meaning that, Conditionally rendering Like Specific Value change that time render specific componanent etc..
		- Include the @if-else, @for @empty,@switch.
		
		1)Ex.
			Conditionally display content with @if, @else-if and @else
			ts :
			@Component({
			  selector: 'app-root',
			  templateUrl: './app.html'
			})
			export class AppComponent {
			  isLoggedIn = false;
			}
			
			Html : 
			@if (isLoggedIn) {
				<h3>Welcome User</h3>
			} @else {
			  <h3>Please Login First</h3>
			}
		
			Referencing the conditional expression's result
			The @if conditional supports saving the result of the conditional expression into a variable for reuse inside of the block.
			
			Ex.
				@if (user.profile.settings.startDate; as startDate) {
				  {{ startDate }}
				}
		
		2) Repeat content with the @for block : 
			
			Ex.
				ts :
				@Component({
				  selector: 'app-root',
				  templateUrl: './app.html'
				})
				export class AppComponent {
				  users = [
					{ id: 1, name: "Sujal" },
					{ id: 2, name: "Jay" },
					{ id: 3, name: "Raj" },
				  ];
				}
				
				html:

				<h4>User List:</h4>
				@for (u of users; track u.id) {
				  <p>{{ u.id }} - {{ u.name }}</p>
				}
				
			Contextual variables in @for blocks
				Inside @for blocks, several implicit variables are always available:

				Variable	Meaning
				$count	Number of items in a collection iterated over
				$index	Index of the current row
				$first	Whether the current row is the first row
				$last	Whether the current row is the last row
				$even	Whether the current row index is even
				$odd	Whether the current row index is odd
			Ex.
				@for (item of items; track item.id; let idx = $index, e = $even) {
				  <p>Item #{{ idx }}: {{ item.name }}</p>
				}

			Providing a fallback for @for blocks with the @empty block
				Ex.
					@for (item of items; track item.name) {
					  <li> {{ item.name }}</li>
					} @empty {
					  <li> There are no items. </li>
					}// if the for loop has no data the @empty render
			
		3) Ex.
			Conditionally display content with the @switch block : 
				ts :
				@Component({
				  selector: 'app-root',
				  templateUrl: './app.html'
				})
				export class AppComponent {
					status = 'loading';
				}
				
				html :
				@switch (status) {
				  @case ('loading') {
					<p>Loading...</p>
				  }
				  @case ('ready') {
					<p>Ready to use!</p>
				  }
				  @default {
					<p>Unknown Status</p>
				  }
				} // here if the value is Loading then case Loading Render Other wise other render or bedefault is render is @default.
				
				
○ Pipes :
		- Pipes are a special operator in Angular template expressions that allows you to transform data declaratively in your template. Pipes let you declare a transformation function once and then use that transformation across multiple templates. Angular pipes use the vertical bar character (|).
		
		Ex.
			@Component({
			  selector: 'app-root',
			  imports: [CurrencyPipe, DatePipe, TitleCasePipe],
			  template: `
				<main>
				   <!-- Transform the company name to title-case and
				   transform the purchasedOn date to a locale-formatted string -->
			<h1>Purchases from {{ company | titlecase }} on {{ purchasedOn | date }}</h1>
					<!-- Transform the amount to a currency-formatted string -->
				  <p>Total: {{ amount | currency }}</p>
				</main>
			  `,
			})
			export class ShoppingCartComponent {
			  amount = 123.45;
			  company = 'acme corporation';
			  purchasedOn = '2024-07-08';
			}
			
			output :
				<h1>Purchases from Acme Corporation on Jul 8, 2024</h1>
				<p>Total: $123.45</p>
		
		◘ Built-in Pipes :
			- it's buit in pipes that help to perform multiple transformation like uppercase, Date, lowercase, tittlecase, slice etc..
			
			| Pipe Name          | Description | ------------------ | ------------------------------------------------------------------ |
			| **AsyncPipe**      | Promise અથવા Observable માંથી value auto subscribe કરીને વાંચે છે. |
			| **CurrencyPipe**   | Number ને currency format માં બતાવે (locale મુજબ).                 
			| **DatePipe**       | Date ને locale rules મુજબ format કરે છે.                           
			| **DecimalPipe**    | Number ને decimal-point સાથે formatted string માં ફેરવે છે.        
			| **I18nPluralPipe** | Plural words (singular/plural) locale મુજબ બદલવા માટે.             
			| **I18nSelectPipe** | Key આધારિત custom string select કરવા માટે.                        
			| **JsonPipe**       | Object ને JSON string રૂપે બતાવે (debugging માટે).                 
			| **KeyValuePipe**   | Object અથવા Map ને key-value pairs ની array માં convert કરે છે.    |
			| **LowerCasePipe**  | Text ને lowercase માં ફેરવે છે.                                    
			| **PercentPipe**    | Number ને percentage format માં દેખાડે છે.                         
			| **SlicePipe**      | Array અથવા String માંથી ચોક્કસ portion (slice) કાઢવા માટે.         
			| **TitleCasePipe**  | Text ને Title Case માં ફેરવે છે.                                   
			| **UpperCasePipe**  | Text ને uppercase માં ફેરવે છે.                                    
	
		Ex.
			Combining multiple pipes in the same expression
				<p>The event will occur on {{ '2024-08-27' | date | uppercase }}.</p>
			
			Passing parameters to pipes
				<p>The event will occur at {{ '2024-08-27' | date:'hh:mm' }}.</p>
				<p>The event will occur at {{ '2024-08-27' | date:'hh:mm':'UTC' }}.</p>
			
			Pipe operator precedence :The pipe operator has lower precedence than other binary operators, including +, -, *, /, %, &&, ||, and ??.
				<!-- firstName and lastName are concatenated before the result is passed to the uppercase pipe -->
				{{ firstName + lastName | uppercase }}
				{{ (isAdmin ? 'Access granted' : 'Access denied') | uppercase }}
				{{ isAdmin ? 'Access granted' : 'Access denied' | uppercase }}
				{{ isAdmin ? 'Access granted' : ('Access denied' | uppercase) }}
				
		○ Creating custom pipes :
			- create a user Declare Pipes by making one single file. Fllow Steps:
				import { Pipe, PipeTransform } from "@angular/core";
				@Pipe({
				  name : 'SujalCustom'
				})

				export class CustomPipe implements PipeTransform{
				  transform(value: any) {
					return value.slice(1,5);
				  }
				} // here main work is PipeTransform interface which helping to us for custom pipes.
				
				html : 
					<p>{{'Sujalgopani' | SujalCustom}}</p> // it's return slice of main string from 1 to 5.
				
			Adding parameters to a custom pipe :
				import { Pipe, PipeTransform } from "@angular/core";
				@Pipe({
				  name : 'SujalCustom'
				})

				export class CustomPipe implements PipeTransform{
				  transform(value: any,format:string) {
					if(format === 'upper'){
					  return value.toUpperCase();
					}else{
					  return value.toLowerCase();
					}
				  }
				} // here format is the parameters varible.
				
				html :
					<p>{{"SujalGopani" | SujalCustom:'upper'}}</p>
				// here the SujalGopani is coverted in to SUJALGOPANI because of parameters.

	○ Custom Behavior Style :
		Component :
			@Component({
			  selector: 'button[baseButton]',
			  styleUrl: './parent.css',
			  standalone : true,
			   template: `
			  <ng-content />
			  `
			})
			
			html:
				<h1>App Component</h1>
				<button baseButton>
				  Sujal <hr>
				  Gopani <hr>
				  Surat
				</button>

---------------
28/11/2025
----------

○ Create template fragments with ng-template :
	- ng-template is a invisible html part which is not shown on a run time.
	- but when somwthing happen like button click, mouse hover etc that time template render on the render area.
	- <ng-template></ng-template> is available in the html file and it's reference is Templateref which is available in componanent side.
	- ng-template :- it's a empty html box when user want that time template render on run time.
	Ex.(simple Example)
		App.ts
		export class AppComponent {
		  isOnline = true;

		  toggleStatus() {
			this.isOnline = !this.isOnline;
		  }
		}
			
		App.html
			<div>
			  <h2>User Status:</h2>

			  <ng-container *ngIf="isOnline; else userOffline">
				<p style="color: green;">🟢 User is Online</p>
			  </ng-container>

			  <!-- else mate no template -->
			  <ng-template #userOffline>
				<p style="color: red;">🔴 User is Offline</p>
			  </ng-template>
			</div>

			<button (click)="toggleStatus()">Toggle Status</button>
			// here by default render the User Online but when the user click button then Rendering is change and render User Offline.
		
		Ex. (ngTemplateoutlet)
			App.ts
				@Component({
					selector: 'app-root',
					imports: [RouterOutlet, ReactiveFormsModule, FormsModule,NgTemplateOutlet],
					templateUrl: './app.html',
					styleUrl: './app.css',
					})
			App.html
			<ng-container *ngTemplateOutlet="sj">
			</ng-container>
			
			<ng-template #sj>
			  <p>testing</p>
			</ng-template>
			// ng-container adapt the ng-template
	
	○ Getting a reference to a template fragment :
		- in this, topic Templateref is the reference of the ng-template.
		- by Templateref we can inject the html template in on the run time.
		
		Ex. (simple (Without name))
			@Component({
			  /* ... */,
			  template: `
				<p>This is a normal element</p>

				<ng-template>
				  <p>This is a template fragment</p>
				</ng-template>
			  `
			})
			export class ComponentWithFragment {
			  templateRef = viewChild<TemplateRef<unknown>>(TemplateRef);
			}
			// here template has no name so templateRef is reference to by default first template.
			
			
		Ex. (with the name)
			@Component({
			  /* ... */,
			  template: `
				<p>This is a normal element</p>

				<ng-template #fragmentOne>
				  <p>This is one template fragment</p>
				</ng-template>

				<ng-template #fragmentTwo>
				  <p>This is another template fragment</p>
				</ng-template>
			  `,
			})
			export class ComponentWithFragment {
			  fragmentOne = viewChild<TemplateRef<unknown>>('fragmentOne');
			  fragmentTwo = viewChild<TemplateRef<unknown>>('fragmentTwo');
			}// here make a reference by it's name.
			
		Ex.(Simple Practical Example Using NgTemplateOutlet)
			App.ts
				import { Component, TemplateRef, viewChild } from '@angular/core';
				@Component({
				  selector: 'app-root',
				  templateUrl: './app.component.html'
				})
				export class AppComponent {

				  helloTemplate = viewChild<TemplateRef<unknown>>('helloTemplate');
				  byeTemplate = viewChild<TemplateRef<unknown>>('byeTemplate');

				  currentTemplate!: TemplateRef<any>;

				  show(num: number) {
					this.currentTemplate = num === 1 ? this.helloTemplate()! : this.byeTemplate()!;
				  }
				}
			
			App.html
				<button (click)="show(1)">Show First Template</button>
				<button (click)="show(2)">Show Second Template</button>

				<ng-container *ngTemplateOutlet="currentTemplate"></ng-container>

				<ng-template #helloTemplate>
				  <h3>👋 Hello Sujal!</h3>
				</ng-template>

				<ng-template #byeTemplate>
				  <h3>👋 Goodbye Sujal!</h3>
				</ng-template>


	○ Using ViewContainerRef :
		- Ex.
			@Component({
			  /* ... */,
			  selector: 'component-with-fragment',
			  template: `
				<h2>Component with a fragment</h2>
				<ng-template #myFragment>
				  <p>This is the fragment</p>
				</ng-template>
				<my-outlet [fragment]="myFragment" />
			  `,
			})
			export class ComponentWithFragment { }
			@Component({
			  /* ... */,
			  selector: 'my-outlet',
			  template: `<button (click)="showFragment()">Show</button>`,
			})
			export class MyOutlet {
			  private viewContainer = inject(ViewContainerRef);
			  fragment = input<TemplateRef<unknown> | undefined>();
			  showFragment() {
				if (this.fragment()) {
				  this.viewContainer.createEmbeddedView(this.fragment());
				}
			  }
			}
			
○ Grouping elements with ng-container :
	- ng-container is the one virtual grouping tag in the html file, means ng-container is not visible in the html file but is available inside the file.
	- we can apply Group elements, Apply structural directives (*ngIf, *ngFor), Avoid creating extra HTML tags
	
	Ex. simple
		<!-- Component template -->
		<section>
		  <ng-container>
			<h3>User bio</h3>
			<p>Here's some info about the user</p>
		  </ng-container>
		</section>

		<!-- Rendered DOM -->
		<section>
		  <h3>User bio</h3>
		  <p>Here's some info about the user</p>
		</section>
		
	○ Using <ng-container> to display dynamic contents :
		- make a display dynamic componanent on html page by using ng-container NgComponentOutlet.
		
		Ex.
			@Component({
			  template: `
				<h2>Your profile</h2>
				<ng-container [ngComponentOutlet]="profileComponent()" />
			  `
			})
			export class UserProfile {
			  isAdmin = input(false);
			  profileComponent = computed(() => this.isAdmin() ? AdminProfile : BasicUserProfile);
			}// here isAdmin when true then render AdminProfile otherwise BasicUserProfile.
			
	○ Rendering template fragments :
		- render dynamic template on the run time with the help of the ng-container and NgTemplateOutlet.
		- it's work on template side working not a componanent.
		
		Ex.
			@Component({
			  template: `
				<h2>Your profile</h2>
				<ng-container [ngTemplateOutlet]="profileTemplate()" />
				<ng-template #admin>This is the admin profile</ng-template>
				<ng-template #basic>This is the basic profile</ng-template>
			  `
			})
			// here ng-container is virtual grouping tag, ng-template is a unvisible html render componanent.[ngTemplateoutlet] is a fill or inject ng-template.
			
			export class UserProfile {
			  isAdmin = input(false);
			  adminTemplate = viewChild('admin', {read: TemplateRef});
			  or
			  adminTemplate = viewChild<Templateref<unknow>>('admin');
			  
			  basicTemplate = viewChild('basic', {read: TemplateRef});
			  or
			  adminTemplate = viewChild<Templateref<unknow>>('basic');
			  // above both wrriten code is right but stytle is deferent
			  profileTemplate = computed(() => this.isAdmin() ? this.adminTemplate() : this.basicTemplate());
			}
			// here Templateref is a make reference to html ng-template.
			
	○ Using <ng-container> with structural directives :
		- apply the if condition or apply for loop on the ng-container.
		
		Ex.
			<ng-container *ngIf="permissions == 'admin'">
			  <h1>Admin Dashboard</h1>
			  <admin-infographic></admin-infographic>
			</ng-container>
			// here visible only <h1> or admin-infographic componanent with if condition if permissions are == admin then is's visible otherwise not.
			
			<ng-container *ngFor="let item of items; index as i; trackBy: trackByFn">
			  <h2>{{ item.title }}</h2>
			  <p>{{ item.description }}</p>
			</ng-container>
			// here same ng-container not visible visible only <h2> or <p> apply here for loop.
			
	○ Using <ng-container> for injection :
		- in injection point, ng-container is actual not visible but when the dependency apply on this ng-container so it's automatically consume parent level.
		- suppose in the ng-container have app-parent, or app-child so if ng-container have DI(dependency Injection) so ng-container is automatic consider as a parent level and app-parent, app-child as child.
		
		Ex.
			theme.directive.ts
				import { Directive } from '@angular/core';

				@Directive({
				  selector: '[theme]',
				  standalone: true,
				  inputs: ['mode: theme']   // 👈 very important fix
				})
				export class ThemeDirective {
				  mode: 'light' | 'dark' = 'light';
				}
			
			parent.component.ts
				import { Component, inject } from '@angular/core';
				import { ThemeDirective } from '../DI/theme';

				@Component({
				  selector: 'app-parent',
				  standalone: true,
				  template: `
					<h1>Parent Component → Theme: {{ theme.mode }}</h1>
				  `
				})
				export class Parent {
				  theme = inject(ThemeDirective);
				}
					
			child.component.ts
				import { Component, inject } from '@angular/core';
				import { ThemeDirective } from '../DI/theme';

				@Component({
				  selector: 'app-child',
				  standalone: true,
				  template: `
					<h2>Child Component → Theme: {{ theme.mode }}</h2>
				  `
				})
				export class ChildComponent {
				  theme = inject(ThemeDirective);
				}
	
			app.component.html
				<h1>App Component</h1>

				<!-- Dark Theme Section -->
				<ng-container theme="dark">
				  <app-parent></app-parent>
				  <app-child></app-child>
				</ng-container>

				<hr>

				<!-- Light Theme Section -->
				<ng-container theme="light">
				  <app-parent></app-parent>
				  <app-child></app-child>
				</ng-container>

				<router-outlet />
				
			FINAL OUTPUT (CORRECT)
				App Component
				Parent Component → Theme: dark
				Child Component → Theme: dark
				-------------------------------
				Parent Component → Theme: light
				Child Component → Theme: light
			// here theme is the main parent file it's inject in the parent or child file also so now, in ng-container parent, child are imported in the app but theme are injected also in both file parent & child so here ng-container are automatically become a parent.
	
-------------
01/12/2025
----------
○ variable in the template :
	- Variable is the used for a store the data from ts file, and it's define with @let, it's also updatable.
		@let name = user.name;
		@let greeting = 'Hello, ' + name;
		@let data = data$ | async;
		@let pi = 3.14159;
		@let coordinates = {x: 50, y: 100};
		@let longExpression = 'Lorem ipsum dolor sit amet, constructor adipiscing elit ' + 'sed do eiusmod tempor incididunt ut labore et dolore magna ' + 'Ut enim ad minim veniam...';
		
		Ex.	
			componanent :
			@let name = user.name;
			
			Html : 
			<h2>welcome :  {{ name }}</h2>

	○ Variable scope :
		- @let declarations are scoped to the current view and its descendants. Angular creates a new view at component boundaries and wherever a template might contain dynamic content, such as control flow blocks, @defer blocks, or structural directives.
		
		Ex.
			@let topLevel = value;
			<div>
			  @let insideDiv = value;
			</div>
			{{topLevel}} <!-- Valid -->
			{{insideDiv}} <!-- Valid -->
			@if (condition) {
			  {{topLevel + insideDiv}} <!-- Valid -->
			  @let nested = value;
			  @if (condition) {
				{{topLevel + insideDiv + nested}} <!-- Valid -->
			  }
			}
			{{nested}} <!-- Error, not hoisted from @if --> // it's not accesible because the nested is declare in the under if condition so it's not access out of if border range.
		
	○ Full syntax :
		The @let syntax is formally defined as:

		The @let keyword.
		Followed by one or more whitespaces, not including new lines.
		Followed by a valid JavaScript name and zero or more whitespaces.
		Followed by the = symbol and zero or more whitespaces.
		Followed by an Angular expression which can be multi-line.
		Terminated by the ; symbol.
	
	○ Declaring a template reference variable :
		- in this topic we make a variale with # sign which meaning, we can access variable direct in to ts file.
		Ex.
			ts.file
				export class App {
				   show(msg : any){
					alert(`Hellow ${msg}`);
				   }
				  }
			
			html :
				<input #taskInput placeholder="Enter task name">
				<button (click)="show(taskInput.value)">hShow</button>
			// here taskInput variable access direct to ts file without declare anything, here input value is accesible by just it's variable name.
			
	○ Assigning values to template reference variables :
		- make a reference to other componanent and access those componanent method, or variable or anything to current componanent.
		- like a import our child componanent in the parent componanent and gives to one reference variable to child componanent and eassy accesible all properties to parent componanent.
		 Ex.
			App.ts
				export class App  {
				  childRef = viewChild<Parent>('childRef');

				  callChild() {
					this.childRef()?.showMessage();
				  }
				}
			
			html : 
				<app-parent #childRef></app-parent>
				<button (click)="callChild()">Call Child Method</button>
				// here import parent componanent and give the reference variable and access the that componanent in to parent componanent by viewchid.
				
	○ Using template reference variables with queries :
		- in the same template when we want to access value like a input so it's eassy by '#' but,
		- when we want to access value from template to componanent so it's not possible by '#' only but we use a @ViewChild('#') name! : ElementRef, so here name is make as a intance of the '#',
		Ex.
			Direct Access :
				ts.file 
				show(msg : any){
					alert(msg);
				}
				
				html.file 
					<input placeholder="Enter Name :" #inp/>
					<button (click)="Show(inp.value)">ShowMe</button>
					// it's cover in above side also but again cover, inp.value is direct accesible in the same template.
					
			By Referencing Access :
				ts.file 
					 export class App  {
					  @ViewChild('inp') intp! : ElementRef;
					  @ViewChild('result') txtarea! : ElementRef;
					  Show() {
						  this.txtarea.nativeElement.value = this.intp.nativeElement.value;
						}
					} // here make a inp and result reference to componanent by ViewChild and elementRef. now by this we access all property of reference.
				
				html.file 
						<input placeholder="Enter Name :" #inp/> // give varible name wirth '#'

						<textarea #result></textarea> // same give variable name by '#'
						<button (click)="Show()">ShowMe</button>// click show event
				// so here not possible to accesible the value from template to componanent so used a viewChild and elementRef.
				
○ Deferred loading with @defer :
	- Deferrable views, also known as @defer blocks, reduce the initial bundle size of your application by deferring the loading of code that is not strictly necessary for the initial rendering of a page. This often results in a faster initial load and improvement in Core Web Vitals (CWV), primarily Largest Contentful Paint (LCP) and Time to First Byte (TTFB).
	Ex.
		@defer {
		  <large-component />
		}

	○ How to manage different stages of deferred loading :
		- it's have to sub blocks which is help to improve the user experience.
		
		1. @defer :
			- this is a primary block to define the Deferred.
				Ex.	
					@defer {
					  <large-component />
					}
		2. @placeholder :
			- The @placeholder is an optional block that declares what content to show before the @defer block is triggered.
			Ex.
				@defer {
				  <large-component />
				} @placeholder {
				  <p>Placeholder content</p>
				} // during the loading large-component placeholder is shown on the html page like Loding..., Or any loader.
				
			Ex.(show placeholder minimum specific ms(mimisecond), or s(second))
				@defer {
				  <large-component />
				} @placeholder (minimum 500ms) {
				  <p>Placeholder content</p>
				}// here minimum 500ms placeholder is shown after main loaded componanent is see on the html page.
				
		
		3. @loading :
			- The @loading block is an optional block that allows you to declare content that is shown while deferred dependencies are loading. It replaces the @placeholder block once loading is triggered.
			
			Ex.
				@defer {
				  <large-component />
				} @loading {
				  <img alt="loading..." src="loading.gif" />
				} @placeholder {
				  <p>Placeholder content</p>
				}
			
			Ex.(After, Minimum)
				@defer {
				  <large-component />
				} @loading (after 100ms; minimum 1s) {
				  <img alt="loading..." src="loading.gif" />
				}// here after is delay time and minimum is duration time.
				-> after : after 100ms will loading is affect on html page.
				-> minimum : total or minimum 1 second time duration @loading is affect on html page.
			
		4. @error :
			- During loading the componanent if any error are occur then @error block are render.
				Ex.
					@defer {
					  <large-component />
					} @error {
					  <p>Failed to load large component.</p>
					} // if any error happen then @error render on the html page.
					
	
	○ Controlling deferred content loading with triggers :
		- Many type of the trigger are available in angular, it's affect when the user react to application. like when user hover, click button then componanent loaded otherwise not loaded.
		
		Trigger :
			
			Trigger		Description
			--------------------------------------------------------------
			idle		Triggers when the browser is idle(free).
			viewport	Triggers when specified content enters the viewport
			interaction	Triggers when the user interacts with specified element
			hover		Triggers when the mouse hovers over specified area
			immediate	Triggers immediately after non-deferred content has finished 	rendering(click or keyboard happening by user.)
			timer		Triggers after a specific duration
			
		Ex.	
			1.idle :
				- The idle trigger loads the deferred content once the browser has reached an idle state, based on requestIdleCallback. This is the default behavior with a defer block.

				<!-- @defer (on idle) -->
				@defer {
				  <large-cmp />
				} @placeholder {
				  <div>Large component placeholder</div>
				}
				
			2.viewport :
				- The viewport trigger loads the deferred content when the specified content enters the viewport using the Intersection Observer API. Observed content may be @placeholder content or an explicit element reference.

				By default, the @defer watches for the placeholder entering the viewport. Placeholders used this way must have a single root element.

					@defer (on viewport) {
					  <large-cmp />
					} @placeholder {
					  <div>Large component placeholder</div>
					}
					
					<div #greeting>Hello!</div>
					@defer (on viewport(greeting)) {
					  <greetings-cmp />
					} // greetings-cmp when render when user scrolled down to <div #greeting>
					
			3.interaction :
				- The interaction trigger loads the deferred content when the user interacts with the specified element through click or keydown events.
				
				@defer (on interaction) {
				  <large-cmp />
				} @placeholder {
				  <div>Large component placeholder</div>
				} // large-cmp is loaded when the user click the placeholder text.
				
				<div #greeting>Hello!</div>
				@defer (on interaction(greeting)) {
				  <greetings-cmp />
				}// large-cmp is loaded when the user click the #greeting div text.
				
			4.hover :
				-The hover trigger loads the deferred content when the mouse has hovered over the triggered area through the mouseover and focusin events. 
				
				@defer (on hover) {
				  <large-cmp />
				} @placeholder {
				  <div>Large component placeholder</div>
				} // large-cmp loaded when the user hover on placeholder.
				
				<div #greeting>Hello!</div>
				@defer (on hover(greeting)) {
				  <greetings-cmp />
				} // large-cmp loaded when the user hover on <div>.
		
			5.immediate :
				The immediate trigger loads the deferred content immediately. This means that the deferred block loads as soon as all other non-deferred content has finished rendering.

				@defer (on immediate) {
				  <large-cmp />
				} @placeholder {
				  <div>Large component placeholder</div>
				} // loaded immediate without waiting anybudy !.
				
			6.timer :
				The timer trigger loads the deferred content after a specified duration.

				@defer (on timer(500ms)) {
				  <large-cmp />
				} @placeholder {
				  <div>Large component placeholder</div>
				} // 500ms after loaded.
				
		○ when : 
			The when trigger accepts a custom conditional expression and loads the deferred content when the condition becomes truthy.

			@defer (when condition) {
			  <large-cmp />
			} @placeholder {
			  <div>Large component placeholder</div>
			} // conditional based rendering.
			
		○ Prefetching data with prefetch :
		 - when we want to data is loaded on background and it's load and ready to load.
		 @defer (on interaction; prefetch on idle) {
			  <large-cmp />
			} @placeholder {
			  <div>Large component placeholder</div>
			} // it's mean loaded on interaction, and prefetch with idle(when browser is free).
			
			
-------------
02/12/2025
----------

○ Expression Syntax :
	- Angular Expression Is A Same As A JavaScript Like But Not It's A Subset Of JavaScript.
	
	Expression Mainlly Used Like This :
		{{ user.name }}
		{{ price * qty }}
		{{ isActive ? 'Yes' : 'No' }}
		
		1. Value Literals :
			| Type                | Examples              |
			| ------------------- | --------------------- |
			| **String**          | `'Hello'`, `"World"`  |
			| **Boolean**         | `true`, `false`       |
			| **Number**          | `123`, `3.14`         |
			| **Object**          | `{name: 'Alice'}`     |
			| **Array**           | `['Onion', 'Cheese']` |
			| **null**            | `null`                |
			| **Template string** | `` `Hello ${name}` `` |
			| **RegExp**          | `/\d+/`               |
		
		*Bingint Is Not Supported In Angular Application*
		
		2. Globals in Angular Expressions :
			- $any() Is A Used As a Globals Expression like $any(value).
				○ Other JavaScript globals NOT allowed:
					Number
					Boolean
					parseInt
					NaN
					Infinity
					Math.random
					Date
					(Almost everything else)
					
		3. Local Variables (Special $ variables) :
			- In The Angular @for, @switch Provide Own Special Variable.
			Ex.
				@for (item of items; track $index) {
				  {{ $index }} – {{ item }}
				}
				
				Common $-variables in loops:
					1.$index
					2.$count
					3.$first
					4.$last
					5.$even
					6.$odd
		
		4. Supported Operators :
			✔ Arithmetic
			+, -, *, /, %, **

			✔ Parentheses
			(a + b) * c

			✔ Conditional (ternary)
			age > 18 ? 'Adult' : 'Minor'

			✔ Logical operators
			&&, ||, !

			✔ Comparison
			<, >, <=, >=, ==, !=, ===, !==

			✔ Nullish coalescing
			{{ name ?? 'Guest' }}

			✔ Assignment operators
			=, +=, -=, *=, /=, etc.

			✔ Property access
			person['name']
			
			✔ Pipe operator
			{{ price | currency }}

			✔ Optional chaining (Angular version)
			user?.address?.city
			
			✔ Non-null assertion (!)
			user!.name
		
		○ Unsupported Operators in Angular
			❌ Bitwise operators
			&, |, ^, ~, <<, >>, etc.

			❌ Object destructuring
				Not allowed:
				const { name } = user   ❌
				
		
		Declarations	
			Ex.
				Variables	let label = 'abc', const item = 'apple'
				Functions	function myCustomFunction() { }
				Arrow 		Functions	() => { }
				Classes		class Rectangle { }

○ Directives :
	- Directives are classes that help to apply additional behavior in to element in your angular application.
	
	1.Built-in directives :
		- Some Directive Are Buit In The Angular Which Is Made By Developer In Past Time.
		
		Ex. 
			
			Directive Types			Details
			-------------------------------------------------------------
			Components			  -> Used with a template. This type of 
									 directive is the most common directive type.

			Attribute directives  -> Change the appearance or behavior of an
									 element, component, or another directive.

			Structural directives -> Change the DOM layout by adding and
									 removing DOM elements.
	
	○ Built-in attribute directives :
		- Attribute directives listen to and modify the behavior of other HTML elements, attributes, properties, and components.

		+-----------------+ +-------+
		|Common directives| |Details|
		+-----------------+ +-------+
		|NgClass		  |	|Adds and removes a set of CSS classes.
		|NgStyle		  |	|Adds and removes a set of HTML styles.
		|NgModel		  |	|Adds two-way data binding to an HTML form
		+-----------------+ +--------
		
		
		1. Adding and removing classes with NgClass :
			- Add or remove multiple CSS classes simultaneously with ngClass.
			- Adding NgClass To ts file first.
			Ex.
				App.ts :
					export class App  {
					  currentStyle: Record<string, boolean> = {};
					  isUnchanged: boolean = true;
					  canSave: boolean = true;
					  isSpecial: boolean = true;
					  
					  setCurrentStyle() {
						this.currentStyle = {
						  saveable: this.canSave,       // true → class apply
						  modified: !this.isUnchanged,  // !true → false → class NOT apply
						  special: this.isSpecial,      // true → class apply
						};
					  }

					  Default() {
						this.currentStyle = {
						  saveable: false,       // true → class apply
						  modified: false,  // !true → false → class NOT apply
						  special: false,      // true → class apply
						};
					  }
					}

				App.css :
					.saveable {
					  color: green;
					}
					.modified {
					  font-weight: bold;
					}
					.special {
					  background: yellow;
					}

				App.html :
					<div (mouseenter)="setCurrentStyle()" (mouseleave)="Default()" [ngClass]="currentStyle">Testing !</div>
				// here we make one example which is give to information for Where is NgClass Apply on Angular Application.
				// When you Hover the Div then apply 3 class or mouse leave from area then automatic remove that 3 class.
				
		2. NgStyle :
			- NgStyle Is Used For a adding or removing the inline style.
			- Import NgStyle First.
			Ex.
				App.ts :
					export class App  {
					  currentStyle: Record<string, string> = {};
					  isUnchanged: boolean = false;
					  canSave: boolean = false;
					  isSpecial: boolean = false;

					  setCurrentStyle() {
						this.currentStyle = {
						  'font-style': this.canSave ? 'italic' : 'normal',
						  'font-weight': !this.isUnchanged ? 'bold' : 'normal',
						  'font-size': this.isSpecial ? '24px' : '12px',
						};
					  }
					}

				App.html :
					<div [ngStyle]="currentStyle" (mouseenter)="setCurrentStyle()">Testing !</div>
				// here when the user hover the div when inline css applied.
	
	○ Hosting a directive without a DOM element :
		
		○ <ng-container> :
			- Ng container Is the grouping element that is not visible in the html or angular do not inject into html code.
			- ng container is a mainlly work for a conditionally rendering element. by *ngif, *for etc..
			
			Ex.
				App.ts :										
					export class App {
					  hero = false;
					  name = 'Angular';
					}
					
				App.html :
					<p>
					  I turned to 
					  <ng-container *ngIf="hero">
						{{name}}
					  </ng-container>
					  technology
					</p>
				// here when the hero is true then name value is print otherwise is not show but not affect in the html page beacause of ng-container.
				
		○ Example Of When the User Click The Checkbox For Not Show Surat City When Conditionally in The Select Element Surat Not Visible And When User Not Checked The Checkbox	then All City areVisible : 
			Ex.	
				App.ts	
					export class App {
					  showSad: boolean = false;

					  selectval = [
						{id: 101,City:'Surat'},
						{id:102,City:'Rajkot'},
						{id:103,City:'Bhavnagar'},
						{id:104,City:'Botad'},
						{id:105,City:'Porbandar'},
						{id:106,City:'Bharuch'},
					  ];

					  changeshowsad() {
						this.showSad = !this.showSad;
						console.log(this.showSad);
					  }
					}
			
				App.html :
					<div>
					  Tick Surat To Not show Surat :
					  <input type="checkbox" id="cksurat" [checked]="showSad" (change)="changeshowsad()"  />
					  <label for="cksurat">Not Show Surat</label>
					</div>


					<p *ngFor="let i of selectval">
					  <ng-container *ngIf="!showSad || i.City !=='Surat'">
						{{i.City}}
					  </ng-container>
					</p>
					// here if the user tick then surat not visible otheriwse visible.
					
	○ Attribute directives :
		- Change the appearance or behavior of DOM elements and Angular components with attribute directives.
		
		○ Building an attribute directive :
			1. To create a directive, use the CLI command ng generate directive.
				Ex. -> ng generate directive highlight(directive's name)
				
				1.1. Directive First time Make like :
					import {Directive} from '@angular/core';
					@Directive({
					  selector: '[appHighlight]',
					})
					export class HighlightDirective {}
					// here @Directive selector is a name of css attribute which we used on the html element like a componanent selector.
			
			2. Import ElementRef from @angular/core. ElementRef grants direct access to the host DOM element through its nativeElement property.
			
			3. Add ElementRef in the directive's constructor() to inject a reference to the host DOM element, the element to which you apply appHighlight.
			
			4. Add logic to the HighlightDirective class that sets the background to yellow.
			
			Ex. (Simple Example)
				import {Directive, ElementRef, inject} from '@angular/core';
				@Directive({
				  selector: '[appHighlight]',
				})
				export class HighlightDirective {
				  private el = inject(ElementRef);
				  constructor() {
					this.el.nativeElement.style.backgroundColor = 'yellow';
				  }
				} // here we make, where we placed the appHighlight there backgroundColor is convert in to yellow.
				
				In the app Or Any html File :
					<p app:Highlight>This is invalid</p>
					// here <p></p> is converted into with backgroundColor yellow.
				
		○ Handling user events :
			- This section shows you how to detect when a user mouses into or out of the element and to respond by setting or clearing the highlight color.
			
			1. Configure host event bindings using the host property in the @Directive() decorator.
			
				import {Directive, ElementRef, inject} from '@angular/core';
				@Directive({
				  selector: '[appHighlight]',
				  host: {
					'(mouseenter)': 'onMouseEnter()',
					'(mouseleave)': 'onMouseLeave()',
				  },
				})
				export class HighlightDirective {
				  private el = inject(ElementRef);
				  onMouseEnter() {
					this.highlight('yellow');
				  }
				  onMouseLeave() {
					this.highlight('');
				  }
				  private highlight(color: string) {
					this.el.nativeElement.style.backgroundColor = color;
				  }
				}    
				// here we make a when user mouse is hovered in to area then backgroundColor is yellow and after leave the area then simple white it's possible with the host.
		
		○ Passing values into an attribute directive :
			- When we want to directive value passed on the main html page load time then we apply the input() in the directive file code.
			- other side we use the constructor use but here we use the ngOnInit() method.
			 
			Ex.
				Highlight.ts
					import { Directive,ElementRef, inject, input } from '@angular/core';
					@Directive({
					  selector: '[appHighlight]'
					})

					export class Highlight {
					  appHighlight = input('');  
					  private p1 = inject(ElementRef);
					  ngOnInit() {
						this.p1.nativeElement.style.backgroundColor = this.appHighlight();
					  }
					}
					
				App.ts :						
					export class App {
					  color = 'red';
					}
				
				App.html :
					<p [appHighlight]="color">Testing</p>
			
		○ Setting the value with user input :
			- Any Conditional if the user wamt to change directive value then do this.
			Ex.
				<div>
				  <input type="radio" name="colors" (click)="color='lightgreen'">Green
				  <input type="radio" name="colors" (click)="color='yellow'">Yellow
				  <input type="radio" name="colors" (click)="color='cyan'">Cyan
				</div> 
				// other code is same only change app.ts variable value which passed in the directive input.
				
		
		○ Binding to a second property :
			- Add One More Input for Second Input Binding.
				Ex.
					  appHighlight = input('');<-(That is first property)
					  defaultColor = input('');<-(Now Add one More)
					  
					Write In the method Like :
						onMouseEnter() {
							this.highlight(this.appHighlight() || this.defaultColor() || 'red');
						  }
					// here if the first property not bind then second property can work and also if second property is not bind then third static value is work.
					
		
		○ Deactivating Angular processing with NgNonBindable :
			- sometime we want to angular can not affect on own html code.
				Ex. {{6+1}}
				If We Write This in the html code then angular convert the 7 but we want to angular is not affect here and show this like same {{6+1}} as it is so use NgNonBindable.
				
				Ex.
					<p>{{ 1 + 1 }}</p> // here angular work and convert into 2
					<p ngNonBindable>{{ 1 + 1 }}</p> // here print as it is.
				// in short we turn off angular by NgNonBindable attribute.
				
				
○ Structural directives:
	- Structural directives are directives applied to an <ng-template> element that conditionally or repeatedly render the content of that <ng-template>.
	- in this topic we cover the how the inject the html in to ng-template by using structural directive.
	- some built in directive like *ngif,*for,*ngswitch etc are use eassy in the html code With '*'.
	- without '*' this when we apply the directive in the html code that time angular consider as a attribute directive, same thing used with the '*' so angular consider as a structural directive.
	Ex.
		Suppose we have a one directive named : 'select'.
		When used in the html,
		
		<p select>Testing ! </p>
		// here angular consider this html code as a attribute directive.
		
		but,
		<p *select>Testing ! </p>
		// here angular consider this line a as a structural directive.
		// one main thing angular convert this line into
		<ng-template select>
			<p>Testing ! </p>
		</ng-template>
		
		-> so both code are same on the compilation time simple <p> with '*' and <ng-template>
		-> ng-template not visible in the html page when we want to that visible so use TemplateRef with ViewContainerRef.
		
		Let's see the one example to better understanding the what is structural directive mean ? here we see custom structural directive.
		
		Ex.	
			import { Directive, ElementRef, inject, Input, TemplateRef, ViewContainerRef } from '@angular/core';
			
			class DataBind{
			  $implicit : string | undefined
			  tech : string | undefined
			}

			@Directive({
			  selector: '[select]'   // structural directive
			})
			export class Highlight {
			 
			constructor(
			  private readonly elemetnref : ElementRef, // for access html page
			  private readonly template : TemplateRef<DataBind>,// for ng-template
			  private readonly vcr : ViewContainerRef // for inject template into container
			){}

			  ngOnInit():void{
				  this.vcr.clear
				  this.vcr.createEmbeddedView(this.template,{$implicit : 'Sujal',tech : 'Angular'});
			  }
			}// this directive help to inject ng-template into ng-container
			
			html :
				<ng-template select let-t="tech" let-u>
				  <h1>Testing From {{t}} By {{u}}</h1>
				</ng-template>
				// in simple ng-template not visible in the html page
				// but apply select named directive it's visible or inject into container.
				// also here pass the value from the directive
				// and if we want to pass value from app.ts then used @Input() in directive file.
		
	○ Directive composition API :
		- when one directive is ready for perform task on component but we want to one directive perform multiple componanent,
		- that's time indivisual work not possible on multiple componanent and it's also long or lazr way to do work between one directive and multiple componanent.
		- so here used a 'hostDirectives' is used.
		- in this simple world one single directive import in the hostDirectives and apply all power of directive in componanent automatically. better understanding with example.
		
		Ex.
			highlight.ts (directive):
				import { Directive, HostBinding, HostListener} from '@angular/core';
				@Directive({
				  selector: '[highlight]',
				  standalone: true
				})
				export class Highlight {

				  @HostBinding('style.background')
				  bg = 'transparent';

				  @HostListener('mouseenter')
				  onEnter() {
					this.bg = 'Yellow';
				  }

				  @HostListener('mouseleave')
				  onLeave() {
					this.bg = 'transparent';
				  }
				}
				
			Parent.ts (thirld party componanent) :
				import { Component } from '@angular/core';
				import { Highlight } from '../Directives/highlight';

				@Component({
				  selector: 'app-parent',
				  standalone: true,
				  template: `<p>Hover me!</p>`,
				  host: {
					'style': 'display:block; padding:20px;'
				  },
				  hostDirectives: [Highlight]
				})
				export class Parent {}
			
			app.html :
				// here on local html we can write manually highlight for perform directive power on local componanent html
				<h2 highlight>Testing !</h2>
					
				// here not write the something like highlight for apply power of componanent.
				// simple we can add the highlight directive in parent componanent in hostDirectives.
				<app-parent></app-parent>

		○ Including inputs and outputs :
			- some condition we pass value in directive from componanent then used the input or output.
			- simple used in hostDirectives available inputs & outputs.
			- better understanding by example.
			Ex.(apply only input on below example)
				
				highlight.ts :
					export class Highlight {
					  @Input('UserInput') inputval?: string;
					}

				parent.ts :
					@Component({
					  selector: 'app-parent',
					  standalone: true,
					  template: `<p>Hover me!</p>`,
					  host: {
						'style': 'display:block; padding:20px;'
					  },
					  hostDirectives: [{
						directive : Highlight,
						inputs : ['UserInput'] // use inputs to give value to directive
					  }]
					})
				
				app.html :
					<h2>App Component</h2>
					
					// here give the input value by local html 'red'
					<h2 highlight [UserInput]="'red'">Testing !</h2>
					
					// here give the input value by app-parent
					<app-parent UserInput="pink"></app-parent>
					<router-outlet></router-outlet>

		○ Dependency Injection :
			- if any services available in the angular application so by using inject we used the services in the directive.
			- better  understanding consider the example.
			Ex.
				service File :
					import { Injectable } from '@angular/core';
					@Injectable({ providedIn: 'root' })
					export class LoggerService {
					  log(msg: string) {
						console.log('Logger:', msg);
					  }
					}
				
				directive File :
					// inject manually
					private logger = inject(LoggerService);
					 @HostListener('mouseenter')
					  enter() {
						this.bg = 'yellow';
						// injeactable service use method in the directive
						this.logger.log('Mouse entered from Highlight');
					  }
					  
-------------
04/12/2025
----------
	○ Getting started with NgOptimizedImage :
		- Normal image injection is not a proper way to work with the angular, but here NgOptimizedImage provide the fill, Priority, ngSrc.
		- ngSrc => imrove the LCP scrore.
		- fill => Automatically Put the element in the parent component but parent componanent must be position as a relative, fixed or abosulate.
		- Priority => It's Attribute describe the information to angular that this image is most important when all image is loade on browser.
		
-------------
05/12/2025
----------
○ DI Continues :
	- DI Stand for a Dependency Injection In this meaning, Making one service whichi is not able to make own object in componanent, so one angular is make object for those service by calling in component or inject in current componanent.
	- By Inject We Can Inject Service In Curretn Component,But One Thing Inject Is Used if service class has @Injectable().
	- DI Is Perofrm Many Way :
		 // ✅ In class field initializer
			private service = inject(MyService);
			
		// ✅ In constructor body
		  private anotherService: MyService;
		  constructor() {
			this.anotherService = inject(MyService);
		  }// if used constructor follow this.
		
		export const authGuard = () => {
		  // ✅ In a route guard
		  const auth = inject(AuthService);
		  return auth.isAuthenticated(); // Used Injectable Class Method
		}
	
	Ex.
		Service : // here make one simple service
			import { Injectable } from '@angular/core';
				@Injectable({ providedIn: 'root' }) // here root meaning is service intance is make for singleton
				export class AnalyticsLogger {
				  trackEvent(category: string, value: string) {
					console.log('Analytics event logged:', {
					  category,
					  value,
					  timestamp: new Date().toISOString()
					})
				  }
				}
				
		App.ts :
			export class App {
			  private logger = inject(AnalyticsLogger);
			 
			  load(){
				this.logger.trackEvent('Category','Value');
			  }
			}
		Html :
			<button (click)="load()">Click<button>
			// here we inject the service and apply in the app.ts when we click button then service method run and print on console.
		
					
	○ Creating and using services :
		- Services are reusable pieces of code that can be shared across your Angular application. They typically handle data fetching, business logic, or other functionality that multiple components need to access.
		- in short service is the ready mate thing which is used by multiple componanent.
		
		Creating a service : (making service command)
			ng generate service CUSTOM_NAME
			// this file is created in the open SRC folder if make custom so used the @Injectable.
		
			Ex.
				service :
					import { Injectable } from '@angular/core';
					@Injectable({
					  providedIn: 'root', // make singleton intance
					})
					export class Testservice {
					  private data:string[] = [];

					  adddata(item:string): void{ // add item
						this.data.push(item);
						console.log("Item Added : ",item);
					  }

					  getdata(){ // return all item
						return [...this.data];
					  }

					}
				
				app.ts :
					export class App {
					  datastore = inject(Testservice); // inject service
					  @ViewChild('intem') ite? : ElementRef<HTMLInputElement>;
					  allldata:string[] = [];

					  additem(){
						  this.datastore.adddata(this.ite?.nativeElement.value || '') // call service adddata method
						  this.allldata = this.datastore.getdata() // second method
					  }
					}
				app.html :
					<h2>App Component</h2>
					<label>Ente Item :</label>
					<input type="text" placeholder="item Name" #intem>

					<button (click)="additem()">Add Item</button>

					<ul>
					  <li *ngFor="let data of allldata">
						  {{data}}
					  </li>
					</ul>
					<router-outlet></router-outlet>
				
	○ Defining dependency providers :
		- Angular provides two ways to make services available for injection:
		- Automatic provision - Using providedIn in the @Injectable decorator or by providing a factory in the InjectionToken configuration Manual provision - Using the providers array in components, directives, routes, or application config.
		
		- when we used the Injectable with providedIn root the injection time we just put in the inject() method to inject service.
		- but when used the Only @Injectable() that time we declare the service in the providers in componanent.
			
		
	○ What is an InjectionToken? :
		- in simple way we use the Inject for inject service in the component, but when we inject the value, object, function, constant that time InjectionToken is used.
		- in the componanent we inmport in the providers and give that token value by useValue.
		Ex.
			token file :
				import { InjectionToken } from "@angular/core";
				export const Api_key = new InjectionToken<number>('Api_Key');
				// return the string Api_key which is number type
				
			app.ts :
				@Component({
				  selector: 'app-root',
				  standalone: true,
				  imports: [RouterOutlet],
				  templateUrl: './app.html',
				  styleUrls: ['./app.css'],
				  providers:[{
					provide:Api_key,useValue : 15241 // tokem value assigned
				  }]
				})
				
				export class App {
				  constructor(@Inject(Api_key) private api_key: number){} // inject in constructor
				  clll(){
					console.log("Api Call By : ",this.api_key);
				  }
				} // when we click button then token valus is print which is passed from providers useValue.
				
	○ InjectionToken with providedIn: 'root' :
		- in this topic we just placed the service in the inject(service_name).
		- providedIn : 'root' meaning is make a intance a singleton for whole application.
		- simple making services and inject in the componanent and used it's method or anything.
		
		Ex.
			testservice.ts :
				export class testservice{
					show(){
						console.log("Service Injected !")
					}
				}
			
			App.ts :
				export class App {
					services = inject(testservice);
					apply(){
						services.show();
					}
				}
			
	○ Creating component-specific instances :
		- if create the DI class or token in the same componanent then it's must be declare the in the providers beacause when service is created in the same componanent then it's override as result service is destroy at the componanent level.
			Ex.
				app.ts :
					@Injectable({ providedIn: 'root' }) // sevice class
						export class DataStore {
						  private data: ListItem[] = [];
						}
						
					@Component({
					  selector: 'app-isolated',
					  // Creates new instance of `DataStore` rather than using the root-provided instance.
					  providers: [DataStore], // providers declare must be for same componanent
					  template: `...`
					})

					export class app {
					  dataStore = inject(DataStore); // Component-specific instance
					}
	
	○ Injector hierarchy in Angular :
		- in the step by step flow of the value which is flow on top to bottom, example here make one tree first make root service named socialapp, has two service named userprofile, friendlist and here friendlist has a frinedentry service here userprofile has no any sevice.
		- SocialApp can provide values for UserProfile and FriendList
		- FriendList can provide values for injection to FriendEntry, but cannot provide values for injection in UserProfile because it's not part of the tree
		
	○ Declaring a provider :
		- angular DI system is work as a key value pair.
		
		○ Provider configuration object :
			- Every provider configuration object has two primary parts:
			
			Provider identifier: The unique key that Angular uses to get the dependency (set via the provide property)
				◘ Value: The actual dependency that you want Angular to fetch, configured with different keys based on the desired type:
				◘ useClass - Provides a JavaScript class
				◘ useValue - Provides a static value
				◘ useFactory - Provides a factory function that returns the value
				◘ useExisting - Provides an alias to an existing provider
			
			Ex.
				Service File :
					import { InjectionToken } from "@angular/core";
					
					// Useclass
					export class Services{
					  log(){
						console.log("Log are injected !");
					  }
					}

					// Usevalue
					export const USEVALUES= new InjectionToken<string>('USEVALUES');

					// UseFactory
					export const Ram_Num = new InjectionToken<number>('RandomNumber');

					// UseAlis
					export class Aliastype{
					  send(){
						console.log("I am Alias !");
					  }
					}
					export const AlisVar = new InjectionToken<Aliastype>('aliastypes');
			
			App.ts :
				import { Component, ElementRef, Inject, inject, ViewChild } from '@angular/core';
				import { RouterOutlet } from '@angular/router';
				import {  Aliastype, AlisVar, Ram_Num, Services, USEVALUES } from './Service/testservice';

				@Component({
				  selector: 'app-root',
				  standalone: true,
				  imports: [RouterOutlet],
				  templateUrl: './app.html',
				  styleUrls: ['./app.css'],
				  providers:[{
					provide : Services,
					useClass : Services
				  },
				  {
					provide : USEVALUES,useValue : 'USE Value Is USed !'
				  },
				  {
					provide : Ram_Num,useFactory:()=>Math.floor(Math.random()*100)
				  },
				  {
					provide : Aliastype, useClass: Aliastype
				  },
				  {
					provide : AlisVar,useExisting:Aliastype
				  }
				]
				})

				export class App {
				  constructor(private logr : Services){}
				  Usefacultoryse= inject(Ram_Num);
				  UseExistingval = inject(AlisVar);

				  Useclass(){
					this.logr.log();
				  }

				  Usevalue(){
					console.log(USEVALUES.toString());
				  }

				  UseFactory(){
					console.log(this.Usefacultoryse);
				  }

				  UseExiting(){
					this.UseExistingval.send();
				  }
				}
			
			App.html :
				<h2>App Component</h2>
				<button (click)="Useclass()">Use Class</button>
				<button (click)="Usevalue()">Use Value</button>
				<button (click)="UseFactory()">Use Factory</button>
				<button (click)="UseExiting()">Use Existing</button>
				<router-outlet></router-outlet>

-------------
08/12/2025
----------
	○ Provider identifiers :
		- 

	○ Injection context :
		- The dependency injection (DI) system relies internally on a runtime context where the current injector is available.
		- This means that injectors can only work when code is executed in such a context.
		- In short DI is Available When The Angular Manually Inject the Service in the Constructor or class.
			Ex.
				○ Constructor
				@Injectable()
					export class MyService {
					  constructor(private http: HttpClient) {
						// inject(HttpClient) yaha available chhe
					  }
					}
				
				○ Field initializer
				@Injectable()
				export class MyService {
				  private logger = inject(Logger); // works here
				}
				
			// here DI is work only runtime mode which angular inject the manually but plain js not allow to work inject in the plain js.
				Ex.
					setTimeout(() => {
					  const logger = inject(Logger); // ❌ ERROR, no injection context
					}, 1000); // here plain js not allow inject
				
	○ Hierarchical injectors :
		- From the top to bottom apply line by line inherit methods and variable.
		- parent inheri child and child inherit main app class so in the app class can access parent or child methods under single class.
			Ex.
				class 1
				class 2 extends 1
				class main extends 2 // here access 2 or also 1 methods or variable.
				
		○ Types of injector hierarchies

			Angular has two injector hierarchies:

				Injector hierarchies	 		Details
				--------------------------------------------------
				EnvironmentInjector hierarchy	Configure an 				EnvironmentInjector in this hierarchy using @Injectable() or providers array in ApplicationConfig.
				
				ElementInjector hierarchy		Created implicitly at each DOM element. An ElementInjector is empty by default unless you configure it in the providers property on @Directive() or @Component().
				
		○ EnvironmentInjector :
			- Gujrati : EnvironmentInjector application-wide root injector chhe, jya services globally available hoy chhe.
			- in short EnvironmentInjector is a provide service as a globally, and that done by two way.
			
				1.@Injectable() providedIn :
					@Injectable({
							  providedIn: 'root'  // root EnvironmentInjector ma available
							})
							export class ItemService {
							  name = 'telephone';
							}
							
				2.ApplicationConfig.providers array (bootstrapApplication() ma) 
					bootstrapApplication(AppComponent, {
					  providers: [
						{ provide: ItemService, useClass: ItemService }
					  ]
					});
				
				Tip:
					providedIn: 'root' = tree-shaking support
					→ Unused services automatically remove thayi → smaller bundle size.
					→ ApplicationConfig.providers = manually override, app-wide providers.
					
		○ ElementInjector :
			- when we use the @componanent / viewproviders so here we create the ElementInjector for this @componanent.
			- Component ni andar je providers define karo → e service aa particular component ma j available.
			-Child components ma availability visibility rules par depend kare.
			- Component destroy thaye tyare service instance pan destroy.
			Ex.
				@Injectable()
					export class ItemService {
					  name = 'default';
					}
				
				Component-level provider:
				@Component({
				  selector: 'app-test',
				  template: `{{ item.name }}`,
				  providers: [
					{ provide: ItemService, useValue: { name: 'lamp' } }
				  ]
				})
				export class TestComponent {
				  constructor(public item: ItemService) {}
				}
				
				
		○ Resolution modifiers :
			- in the DI if the service Or injected class are not present or not available on the local application so here help the Resolution modifiers.
			- or any service related which is injected in the main class so helpfull it.
			
			○ 4 Resolution Modifiers in Angular
				1.@Optional()
				2.@Self()
				3.@SkipSelf()
				4.@Host()
				
			1) @Optional() :
				- if injected service not present so not show error but it's handle and jusr send back null.
	
					
			2) @Self() :
				- check the self component not check the parent componanent.
			
			3) @SkipSelf() :
				- Check not self but check parent componanent if there has no value then return null or use the @Optional because many cases parent componanent has no value then @optional are return null otherwise app will crash.
			
			4) @Host() :
				- Check the last connected or injected or called component and return last host value.
				
			Ex.
				service :
					import { InjectionToken } from "@angular/core";
					export const MSG_TOKEN = new InjectionToken<string>('MessageToken');

				Parent :
					@Component({
					  selector: 'app-parent',
					  template: `
						<h2>Parent Component</h2>
						<app-child></app-child>
					  `,
					  providers: [
						{ provide: MSG_TOKEN, useValue: "Parent Message" }
					  ],
					  imports: [ChildComponent]
					}) // give the token value 
					
				Child :
					@Component({
				  selector: 'app-child',
				  template: `
					<h3>Child Component</h3>
					<button (click)="show()">Show Outputs</button>
				  `,
				  providers: [
					{ provide: MSG_TOKEN, useValue: "Child Message" }
				  ]
				})
				export class ChildComponent {

				  constructor(
					@Self()       @Inject(MSG_TOKEN) public selfMsg: string, // check self componanent
					@SkipSelf()  @Inject(MSG_TOKEN) public parentMsg: string, // check parent componanent
					@Host()      @Inject(MSG_TOKEN) public hostMsg: string, // check
					@Optional()  @Inject("UNKNOWN_TOKEN") public optionalMsg: any // check injected componanent available or not if not return null
				  ) {}

				  show() {
					console.log("SELF:", this.selfMsg);
					console.log("SKIPSELF:", this.parentMsg);
					console.log("HOST:", this.hostMsg);
					console.log("OPTIONAL:", this.optionalMsg);
				  }
				}
				
				app.html :
					<h2>App Component</h2>
					<app-parent></app-parent>
					<router-outlet></router-outlet>
					
	○ Logical structure of the template :
		- Angular injector is not a dom base but it's a structure based.
		- use the <#View> for the Logical Structure.
		- <#View> is the used for the wrap the componanent.child componanent has injected in this <#View>.
		- by virtual structural DI can deside which provider is search.
		- better understanding by example:
		Ex.
			in the componanent :
				<app-parent>
					<app-child></app-child>
				</app-parent>
			
			In the actual angular can consider :
				<app-parent>
					<#View>
					<app-child>
						<#View>
						.. content
						<#View>
					</app-child>
					<#View>
				</app-parent>
				
				- <#View> is the inject or cover or wrap the child componanent.
		
		○ Providing services in @Component() :
			- in the making service with Injectable() so when inject in componanent then declare once time in providers.
			Ex.
				service :
					import { Injectable } from '@angular/core';
					@Injectable()
					export class ItemService {
					  name = 'Lamp';
					}
					
				component :

					@Component({
					  selector: 'app-my',
					  template: `
						<p>Item name: {{ item.name }}</p>
						<button (click)="change()">Change Name</button>
					  `,
					  providers: [
						ItemService   // 👉 Service provided at component level
					  ]
					})
					export class MyComponent {
					  constructor(public item: ItemService) {} // making a instance which help to access the service method or variable.
					  change() {
						this.item.name = 'Table'; // this method is change the name lamp to Table.
					  }
					}
				// in this code when the service is call in onther componanent that time this componanent instance us distroy.

-------------
09/12/2025
----------
	○ Defernce Between   providers & viewProviders :
		- When the service is inject in the component then two thing are consider :
			Declare with providers or declare with a viewProviders
		
		- Meaning of this two @Component Property :
			- 1. providers :
				- Inject main service in the parent level componanent and it's affect in the all parent's child componanent.
				- suppose make one service and inject in the parent componanent so here it's service's method or variable access by all child of this parent.
				- Ex.
					service.ts
					parent -> injected with provider
					parent has (child,co-child)
					-// here service method is access by child,co-child also.
					
					service :
						import { Injectable } from "@angular/core";
						@Injectable()
						export class FlowerService {
						  emoji = '🌺';
						}
						
					parent.ts :
						import { Component, inject } from '@angular/core';
						import { FlowerService } from './flower.service';

						@Component({
						  selector: 'app-parent',
						  template: `
							<p>Parent Flower: {{ flower.emoji }}</p>
							<app-child></app-child>
						  `,
						  providers: [FlowerService]  // 👈 Service provided here
						})
						export class ParentComponent {
						  flower = inject(FlowerService);
						}
					
					child.ts :
						import { Component, inject } from '@angular/core';
						import { FlowerService } from './flower.service';

						@Component({
						  selector: 'app-child',
						  template: `
							<p>Child Flower: {{ flower.emoji }}</p>
						  `
						})
						export class ChildComponent {
						  flower = inject(FlowerService);
						}
					
					Output :
						Parent Flower: 🌺
						Child Flower: 🌺
					// this happen because service is injected with provider
					
					
			2. viewproviders :
				- service is affected in current parent componanent or it's view, but not affect in out side of the parent.
				 Ex.
					service.ts
					parent -> injected with viewProviders
					parent has (child,co-child)
					-// here service method is not access by child,co-child also.(beacause viewProviders).
				
					parent.ts :
						@Component({
						  selector: 'app-parent',
						  template: `
							<p>Parent Flower: {{ flower.emoji }}</p>
							<app-child></app-child>
						  `,
						  viewProviders: [FlowerService]   // 👈 Only parent view can access
						})
						export class ParentComponent {
						  flower = inject(FlowerService);
						}

					child.ts :
						@Component({
						  selector: 'app-child',
						  template: `
							<p>Child Flower: {{ flower.emoji }}</p>
						  `
						})
						export class ChildComponent {
						  flower = inject(FlowerService);
						}
					
					Output :
						Parent Flower: 🌺
						ERROR: No provider for FlowerService in ChildComponent
						
					// in short :
					providers : Component + all child components
					viewProviders : Component + its own view children only
					
		○ Using the providers array :
			- in the @Component property provider has a many key and value pair to help to inject service to componanent.
			- here also available useValue,useExisting,useClass etc... it availability when we want.
				Ex.
					app.ts :
						@Component({
						  selector: 'app-child',
						  templateUrl: './child.component.html',
						  styleUrls: ['./child.component.css'],
						  // use the providers array to provide a service
						  providers: [{provide: FlowerService, useValue: {emoji: '🌻'}}], // here we can chenage the value manually.
						})
						export class ChildComponent {
						  // inject the service
						  flower = inject(FlowerService);
						  ngOnInit(){
							console.log(this.item.testvalue);
						  }
						}
				
				
		○ Using the viewProviders array :
			- it's a similar to provider but rule is follow as a viewProviders.
			
		
		○ Visibility of provided tokens :
			- in this topic angular can search from start to end injected service.
			- it's follow a rule of selfskip,host,optional,self.
			
			Ex.
					flower = inject(FlowerService, { skipSelf: true })
					flower = inject(FlowerService, { skipSelf: true, host: true }); // multiple

					// it's find the own parent not a current componanent.
					
	
	○ Optimizing client application size with lightweight injection tokens :
		- if we make a class or service and inject in the componanent so componanent makes a bundle for inject the service.
		- but instead of use the injectiontoken, beacause it's a lightweight to inject in the component.
		Ex.
			By class sevice :
				@Injectable()
				export class EmojiService {
				  emoji = '🌺';
				}

			By InjectionToken Service :
				export const FLOWER = new InjectionToken<string>('FLOWER', {
				  providedIn: 'root',
				  factory: () => '🌺'
				});
				
				Inject Time :	
				flower = inject(FLOWER);
			// we should use the injectiontoken for lightweight communication between componanenta and service class.
			
			
	
	○ Inject the component's DOM element :
		- in this angular componanent we should't use the third party DOM access tool instead of we should use the ElementRef,
		- ElementRef is the use for a DOM access for current componanent template.
		
		EX.
			import {Directive, ElementRef, inject} from '@angular/core';
				@Directive({
				  selector: '[appHighlight]',
				})
				export class HighlightDirective {
				  private element = inject(ElementRef);
				  update() {
					this.element.nativeElement.style.color = 'red';
				  }
				}// here access DOM and ElementRef can change the style color to red.
				
	
	○ Inject the host element's tag name :
		- When you need the tag name of a host element, inject it using the HOST_TAG_NAME token.
			Ex.
				import {Directive, HOST_TAG_NAME, inject} from '@angular/core';
				@Directive({
				  selector: '[roleButton]',
				})
				export class RoleButtonDirective {
				  private tagName = inject(HOST_TAG_NAME);
				  onAction() {
					switch (this.tagName) {
					  case 'button':
						// Handle button action
						break;
					  case 'a':
						// Handle anchor action
						break;
					  default:
						// Handle other elements
						break;
					}
				  }
				}
				
	○ Resolve circular dependencies with a forward reference :
		- if the we want to pass the reference the class which is actual not define at a runtime but we pass the class reference that time simple method is give a error but ForwardRef is help full.
		- ForwardRef is reduce the cirular depedency.
		Basic :
			providers: [
			  {
				provide: PARENT_MENU_ITEM,
				useExisting: forwardRef(() => MenuItem),
			  },
			],
		Ex.
			It's a Give A Error beacause class is not definr when it's call :
			@Component({
			  selector: 'app-root',
			  standalone: true,
			  template: 'Hello',
			  providers: [
				{
				  provide: TOKEN,
				  useExisting: MyService   // ❌ Error: MyService is not defined yet
				}
			  ]
			})
			export class AppComponent {}

			export class MyService {}

			
			Ex for ForwardRef :
				@Component({
				  selector: 'app-root',
				  standalone: true,
				  template: 'Hello',
				  providers: [
					{
					  provide: TOKEN,
					  useExisting: forwardRef(() => MyService)  // ✅ fixed
					}
				  ]
				})
				export class AppComponent {
				  constructor(@Inject(TOKEN) public service: MyService) {}
				}

				export class MyService {
				  log() {
					console.log("Service working!");
				  }
				}
				
			componanent :
				
				 providers : [
					{
					  provide : tokenRef,
					  useExisting : forwardRef(()=> MyService)
					}
				  ] // here we declare that, now MyService is not availability but we will create.
				 
				
				
------------
10/12/2025
----------
○ Angular Routing :
	- in the angular app or any SPA(single page application) rendering is the main or important topic of the performance side.
	- Because when we click any link that time browser fully load and redirct to onther page but here SPA is apply so routing is the non load or wihtout load and serve more follow.
	- in short Angular Routing is provide the non load reidrection, when user click link that time angular routing serve the componanent.
	- main thing angular serve the componanent on <route-outlet></route-outlet> is a reserve space to render componanent when the rendering is happen.
	- so wihtout space or <route-outlet> routing is not capable to serve the componanent.
	- in the template use the routelink.
	- more content about the routing is below side.
	
	In addition, the Angular Routing library offers additional functionality such as:

		○ Nested routes
		○ Programmatic navigation
		○ Route params, queries and wildcards
		○ Activated route information with ActivatedRoute
		○ View transition effects
		○ Navigation guards
	
	
○ Define routes :
	- Routes serve as the fundamental building blocks for navigation within an Angular app.

	○ What are routes? :
		- In Angular, a route is an object that defines which component should render for a specific URL path or pattern, as well as additional configuration options about what happens when a user navigates to that URL.
		- Here is a basic example of a route:
		Ex.
			import { AdminPage } from './app-admin/app-admin.component';
			const adminPage = {
			  path: 'admin', // path meaning is url is '/admin'
			  component: AdminPage // url admin render AdminPage render
			}
			
	○ Managing routes in your application :
		- in angular application if we want to use the rendering then we should define the path or routing in the app.route.ts file example is below :
			Ex.
				import { Routes } from '@angular/router';
						import { HomePage } from './home-page/home-page.component';
				import { AdminPage } from './about-page/admin-page.component';
				export const routes: Routes = [
				  {
					path: '',
					component: HomePage,
				  }, // empty path render HomePage component
				  {
					path: 'admin',
					component: AdminPage,
				  }, // /admin path render AdminPage componanent
				];
		
	○ Adding the router to your application :
		- if we make the routing so we can set route file as a global in the entire angular application.
		- suppose we make the one routing so we should attach as a global file by this topic.
			Ex.
				import { ApplicationConfig } from '@angular/core';
				import { provideRouter } from '@angular/router';
				import { routes } from './app.routes';
				export const appConfig: ApplicationConfig = { // ApplicationConfig is the config file of the angular
				  providers: [
					provideRouter(routes), // here we make the route as a global level.
					// ...
				  ]
				};
		
	○ Route URL Paths :
		○ Static URL Paths :
			- in this topic static paths means that, in the route file one path property available and it's consume path url name and exactly that name we enter the url so route is render those path componanent.
			Ex.
				○ "/admin"
				○ "/blog"
				○ "/settings/account"
				// here we enter same path value like admin blog if we enter other value then it's given error, here dynamical value not allow so it's called a static URl.
				
		
		○ Define URL Paths with Route Parameters :
			 - here we can define the route with dynamical parameters. 
			 - like a admin?id=10,admin?id=03 etc... here id is dynamical
			 Ex.
				import { Routes } from '@angular/router';
				import { UserProfile } from './user-profile/user-profile';
				const routes: Routes = [
				  { path: 'user/:id', component: UserProfile } 
				]; // here 
			
			○ Valid route parameter names must start with a letter (a-z, A-Z) and can only contain:

				◘ Letters (a-z, A-Z)
				◘ Numbers (0-9)
				◘ Underscore (_)
				◘ Hyphen (-)
			
			○ We can also write the multiple Parameter:
				Ex.
					const routes: Routes = [
					  { path: 'user/:id/:social-media', component: SocialMediaFeed },
					  { path: 'user/:id/', component: UserProfile },
					];
				//	user - base path
					:id - dynamical id 
					:social-media : also dynamic parameter
					
				
		○ Wildcards :
			- When you need to catch all routes for a specific path, the solution is a wildcard route which is defined with the double asterisk (**).
			- here if the path is not found then render componanent which is in the '**', Example if the componanent not render so '**' is render with PageNotFound.
			Ex.
				import { Home } from './home/home.component';
				import { UserProfile } from './user-profile/user-profile.component';
				import { NotFound } from './not-found/not-found.component';
				const routes: Routes = [
				  { path: 'home', component: Home },
				  { path: 'user/:id', component: UserProfile },
				  { path: '**', component: NotFound }
				]; // here user/:id is not availability so NotFound Render By helping '**'.
				
		○ How Angular matches URLs
			- When you define routes, the order is important because Angular uses a first-match wins strategy. This means that once Angular matches a URL with a route path, it stops checking any further routes. As a result, always put more specific routes before less specific routes.	
			-  in the route file many path available but 2 path are same name so angular find first to last find, and at end of the angular render which componanent when it's catch first.
			- one time angular catch the path and componanent so that is not find next path.
			Ex.
				const routes: Routes = [
				  { path: '', component: HomeComponent },              // Empty path
				  { path: 'users/new', component: NewUserComponent },  // Static, most specific
				  { path: 'users/:id', component: UserDetailComponent }, // Dynamic
				  { path: 'users', component: UsersComponent },        // Static, less specific
				  { path: '**', component: NotFoundComponent }         // Wildcard - always last
				];
				
				If a user visits /users/new, Angular router would go through the following steps:

					1.Checks '' - doesn't match
					2.Checks users/new - matches! Stops here
					3.Never reaches users/:id even though it could match
					4.Never reaches users
					5.Never reaches **
					
	○ Loading Route Component Strategies :
		- Understanding how and when components load in Angular routing is crucial for building responsive web applications. 
		- Angular offers two primary strategies to control component loading behavior:

			1.Eagerly loaded: Components that are loaded immediately
			2.Lazily loaded: Components loaded only when needed
			
		○ Eagerly loaded components :
			- in this point componanent render imedietlly that called a Eagerly loaded.
					Ex.
						import { Routes } from "@angular/router";
						import { HomePage } from "./components/home/home-page"
						import { LoginPage } from "./components/auth/login-page"
						export const routes: Routes = [
						  // HomePage and LoginPage are both directly referenced in this config,
						  // so their code is eagerly included in the same JavaScript bundle as this file.
						  {
							path: "",
							component: HomePage
						  },
						  {
							path: "login",
							component: LoginPage
						  }
						] // here when the angular application is run first time then all componanent are import in the browser which route file has here HomePage or LoginPage are import at a time when angular application are run.
						
		○ Lazily loaded components :
			- in this topic all things are diferent from Eagerly loading, when we need that time import or load the componanent in the browser.
			Ex.
				import { Routes } from "@angular/router";
				export const routes: Routes = [
				  // The HomePage and LoginPage components are loaded lazily at the point at which
				  // their corresponding routes become active.
				  {
					path: 'login',
					loadComponent: () => import('./components/auth/login-page').then(m => m.LoginPage)
				  },
				  {
					path: '',
					loadComponent: () => import('./components/home/home-page').then(m => m.HomePage)
				  }
				] // here when hit the login url that time LoginPage are loaded otherwise it's not loaded.
				
		○ Injection context lazy loading :
			- in the route file we can inject the service by inject().
			Ex.
				import { Routes } from '@angular/router';
				import { inject } from '@angular/core';
				import { FeatureFlags } from './feature-flags';
				export const routes: Routes = [
				  {
					path: 'dashboard',
					// Runs inside the route's injection context
					loadComponent: () => {
					  const flags = inject(FeatureFlags);
					  return flags.isPremium
						? import('./dashboard/premium-dashboard').then(m => m.PremiumDashboard)
						: import('./dashboard/basic-dashboard').then(m => m.BasicDashboard);
					},
				  },
				]; // here when we hit the dashboard url that time FeatureFlags is injected and load in the browser.
				
		○ Should I use an eager or a lazy route?

			- There are many factors to consider when deciding on whether a route should be eager or lazy.

			- In general, eager loading is recommended for primary landing page(s) while other pages would be lazy-loaded.


			NOTE: While lazy routes have the upfront performance benefit of reducing the amount of initial data requested by the user, it adds future data requests that could be undesirable. This is particularly true when dealing with nested lazy loading at multiple levels, which can significantly impact performance.
			
		○ Redirects :
			- in this topic when we go to specific url then redirect to other url.
			- suppore we go the /test then we redire To /child.
			Ex.
				export const routes: Routes = [
				  { path: '', component: App }, // default opening
				  { path: 'child', component: ChildComponent }, // child componanent render
				  {path: 'test',redirectTo:'/child'} // try to go test but redirect to /child
				];

		○ Page titles :
			- it's help to give a tittle of the page which is render on browser.
			Ex.
				export const routes: Routes = [
				  { path: '', component: App },
				  { path: 'child', component: ChildComponent,title:'Sujal Gopani' },
				]; // when someone can redirect to child then that page have Sujal Gopani tittle.
				
		○ Using TitleStrategy for page titles :
			- sometime we want to page tittle is look like a custom or same as like a all tittle is seprated by '-' so this topic is apply on all tittle by 'TitleStrategy'.
			-Ex.
				Custom TitleStrategy :
					import { inject, Injectable } from "@angular/core";
					import { Title } from "@angular/platform-browser";
					import { RouterStateSnapshot, TitleStrategy } from "@angular/router";

					@Injectable()
					export class AppTitleStrategy extends TitleStrategy {
					  private readonly title = inject(Title);
					  updateTitle(snapshot: RouterStateSnapshot): void {
						const pageTitle = this.buildTitle(snapshot) || this.title.getTitle();
						this.title.setTitle(`MyAwesomeApp - ${pageTitle}`);
					  }
					}
					
				AppConfig :
					export const appConfig: ApplicationConfig = {
					  providers: [
					   
						provideRouter(routes),
						{provide : TitleStrategy,useClass:AppTitleStrategy}
					  ]
					};


				🔍 Explanation (Gujarati)

				buildTitle(snapshot)
				→ Route मा जे title: नु value सेट छे ए fetch करे
				Example:
				{ path: 'home', title: 'Home Page' }

				अगर title न होय?
				→ Fallback: index.html नु default title

				Finally:
				"MyAwesomeApp - {title}"

				हर page title auto-format थाय.
				
				
------------
11/12/2025
----------
	○ Associating data with routes :
		- in the route file many type of data which is static so we can write those data in the Data properties.
		- which is not changable on the runtime, like pagename, Id, role etc..
		
		Ex.
			const routes: Routes = [
			  {
				path: 'about',
				component: AboutComponent,
				data: { analyticsId: '456' } // here analyticsId is not change in the runtime
			  }]
			// here read the static state consider next chapter there we can read the data.
			
	○ Dynamic data with data resolvers :
		- in this topic sub children url is consider here,
			Ex.
				 {
					path: 'child',
					component: ChildComponent,
					title:'Child',
					data: {staticId : 101} ,
					children:[
					  {path: 'SujalAngular',component : Parent,title:'Sub Parent'},
					  {path: 'Sutex',component : ChildComponent,title:'Sub Child'}
					]
				  }
				// here root or main url is /child this is provider ChildComponent render when user hit the /child.
				- but in the child url sub child availability which called is SujalAngular or Sutex.
				- so 1 subchild is /child/SujalAngular this url is provide the Parent componanent but in the ChildComponent has <router-outlet></router-outlet>
				- so 2 subchild is /child/Sutex this is provide ChildComponent but in the Root Component has <router-outlet></router-outlet>.
				
				Digram :
					Child (Root) -> provide ChildComponent
					<router-outlet></router-outlet> (Must Be Declare If The Apply Sub Child)
						|
					/SujalAngular -> provide Parent Component
					/Sutex 		  -> provide Child Component inside the ChildComponent
					
○ Show routes with outlets :
	
	○ Secondary routes with named outlets :
		- Pages may have multiple outlets— you can assign a name to each outlet to specify which content belongs to which outlet.
		- in this topic when click user routlink and angular provide or render component in the reserved component place.
		- suppose user click the routelink like a 'Go TO Parent' so we can declare the <route-outlet> with name attribure and name is the reserved for the Parent componanent.
		Ex.
			Parent componanent
			
			-app.td
				routelink => Go To Parent
				
				<route-outlet name="Parent-com">
				// when click Routelink then it's componanent render in the Parent-com placed other place not render.
			
			Ex.
			routes file :
			
				import { Routes } from '@angular/router';
				import { App } from './app';
				import { ChildComponent } from './child/child';
				import { Parent } from './parent/parent';


				export const routes: Routes = [
				  { 
					path: '',
					component: App
				  },

				  {
					path: 'child',
					component: ChildComponent,
					outlet:'child-com'
				  },

				  { 
					path: 'parent', 
					component: Parent,
					outlet:'parent-com'
				  },
				  {
					path:'test',
					redirectTo: '/(parent-com:child)'
				  }
				];

				
				app.html:
					Load Parent in named outlet
					<a [routerLink]="[{ outlets: { 'parent-com': ['parent'] } }]">
					  Go to Parent
					</a>
					<hr> 
					Load Child in named outlet 
					<a [routerLink]="[{ outlets: { 'child-com': ['child'] } }]">
					  Go to Child
					</a>

					<router-outlet name="parent-com"></router-outlet>// reserved place for the Parent com
					<hr>
					<router-outlet name="child-com"></router-outlet>// reserved place for the Child com
					// click Go to Child then render child in own reserved place as same as parent componanent
	
	○ Outlet lifecycle events
			There are four lifecycle events that a router outlet can emit:

			Event		Description
			activate	When a new component is instantiated
			deactivate	When a component is destroyed
			attach		When the RouteReuseStrategy instructs the outlet to attach the subtree
			detach		When the RouteReuseStrategy instructs the outlet to detach the subtree
			
			You can add event listeners with the standard event binding syntax:

			<router-outlet
			  (activate)='onActivate($event)'
			  (deactivate)='onDeactivate($event)'
			  (attach)='onAttach($event)'
			  (detach)='onDetach($event)'
			/>
			
	○ Passing contextual data to routed components :
		- in this topic when the specific component render in the reserved place that time router-outlet is give the some static or dynamic value which is accescible by the rendered component.
		Ex.
			routes File :
				export const routes: Routes = [
				  { 
					path: '',
					component: App
				  },
				  {
					path: 'child',
					component: ChildComponent
				  }
				];
				
			app.html :
				
				<a routerLink="/child">Go to Child</a>

				<router-outlet [routerOutletData]="{ theme: 'dark', version: 2 }"></router-outlet>
				// this is the give to the some value to rendered componanent.
				
			childc componanent :
				@Component({
				  selector: 'app-child',
				  template: `
					<h3>Child Component</h3>
					<p>Theme: {{ outletData().theme }}</p>
					<p>Version: {{ outletData().version }}</p>
				  `,
				  imports: []
				})
				
				export class ChildComponent {
					  outletData = inject(ROUTER_OUTLET_DATA) as any;
					  //  this is the hepl to access the value from the routerOutletData
					  // we can perform same as same operaton in the oher componanent by helipng ROUTER_OUTLET_DATA token.
				}
				

-------------
12/12/2025
----------

○ Navigate to routes :
	- The RouterLink directive is Angular's declarative approach to navigation. It allows you to use standard anchor elements (<a>) that seamlessly integrate with Angular's routing system.
	
	-> under this topic use te Routelink in the anchor tag :
		Ex.
			import {RouterLink} from '@angular/router';
			@Component({
			  template: `
				<nav>
				  <a routerLink="/user-profile">User profile</a> //use routerLink insteadof href
				  <a routerLink="/settings">Settings</a>
				</nav>
			  `
			  imports: [RouterLink], // import first in the componanent file
			  ...
			})
			export class App {}
			
			
	○ Using absolute or relative links :
		- Relative URLs in Angular routing allow you to define navigation paths relative to the current route's location. This is in contrast to absolute URLs which contain the full path with the protocol (e.g., http://) and the root domain (e.g., google.com).
		
		Ex.
			<!-- Absolute URL -->
			<a href="https://www.angular.dev/essentials">Angular Essentials Guide</a>
			<!-- Relative URL -->
			<a href="/essentials">Angular Essentials Guide</a>
		
		// in this if the using the relative or abosulate path then use the href not a routerLink.
		
	○ How relative URLs work :
		
		// in this line we give only static string value like static routes as a /child,/parent,/store etc which has no need to combine the dynamic data
		<a routerLink="dashboard">Dashboard</a>
		
		// here consider a static + dynamic data like a /child/101,/parent/201 etc in the end of the number is changable by dynamically.
		<a [routerLink]="['dashboard']">Dashboard</a>
		
		Ex.(Dynamically Data pass Example)
			route.ts :
				 {
					path :'child/:id', // define the route child with dynamic id
					component : Parent
				 }
			
			nav.ts :
				currentUserId = 205
			
			nav.html :
				<a [routerLink]="['child', currentUserId]">Current User</a>
				// when we click here then go to 'http://localhost:4200/child/205' and parent componanent render.
				// one thing consider /child/:id(give by componanent) if the child/:id route is not available then angular application is throw error so first declare the path after use in the <a> tag.
				
			
			Simple Example :
				ts file :
					user=[
						{currentUserId:101,uname:'Sujal',city:'Surat'},
						{currentUserId:102,uname:'Shrey',city:'Rajkot'},
						{currentUserId:103,uname:'Man',city:'Bharuch'},
						{currentUserId:104,uname:'Meet',city:'surat'},
						{currentUserId:105,uname:'Mitesh',city:'Jaypur'}
					  ]
					  
				html file :
					<table>
					  <thead>
						<tr>
						  <th>Id</th>
						  <th>Name</th>
						  <th>City</th>
						  <th>Go To Profile</th>
						</tr>
					  </thead>

					  <tbody>
						<tr *ngFor="let data of user">
						  <td>{{ data.currentUserId }}</td>
						  <td>{{ data.uname }}</td>
						  <td>{{ data.city }}</td>
						  <td><a [routerLink]="['child',data.currentUserId]">Go</a></td>
						</tr>
					  </tbody>
					</table>
					// here like api data display user data by it's dynamically go to /child/id(101,102,103,104,105 etc..)
					
			// if write the multiple dynamical data at a time so :
				
				<a routerLink="/team/123/user/456">User 456</a>
				<a [routerLink]="['/team', teamId, 'user', userId]">Current User</a>”
	
	○ Programmatic navigation to routes :
		- While RouterLink handles declarative navigation in templates, Angular provides programmatic navigation for scenarios where you need to navigate based on logic, user actions, or application state. By injecting Router, you can dynamically navigate to routes, pass parameters, and control navigation behavior in your TypeScript code.
		- if we want to routing is perform by not a routelink but it's happen by programmly by making event so given information below.
		
		◘ router.navigate() :
			- use this router.navigate() we can redirect to specific route without reloading page.
				Ex.
					<button (click)="navigateToProfile()">View Profile</button>

				ts file :
					import { Router } from '@angular/router';
					@Component({
					  selector: 'app-dashboard',
					  template: `
						<button (click)="navigateToProfile()">View Profile</button>
					  `
					})
					export class AppDashboard {
					  private router = inject(Router);
					  navigateToProfile() {
						// Standard navigation
						this.router.navigate(['/profile']);
						// With route parameters
						this.router.navigate(['/users', userId]);
						// With query parameters
						this.router.navigate(['/search'], {
						  queryParams: { category: 'books', sort: 'price' }
						});
						// With matrix parameters
						this.router.navigate(['/products', { featured: true, onSale: true }]);
					  }
					}
				
		○ relativeTo :
			- this is the helpfull to redirct to own  child or own siblings like suppose we are placed now sujal and under the sujal one edit named route define so write the code in side the sujal's componanent to redirect to edit.
			
			Diagram :
				sujal -> render Parent componanent
				  |
				 edit -> render other or our example parent componanent
				 
				 so here two url is available :
					/sujal & /sujal/edit
			
			- so in the sujal meand parent componanent redirct to own child componanent edit so next redirectation code is write in the parent componanent.
			- that code is redirect to sujal child edit url.
			
			Ex.
				route file :
					  {
						path:'sujal',
						component:Parent,
						children:[
						  {path:'edit',component:ChildComponent}
						]
					  }
				
				main file (nav.html) :
					<a routerLink="/sujal">Go Parent</a>

				parent level file :
					@Component({
					  selector: 'app-parent',
					  template: `
						<h2>Parent Component</h2>
						<button (click)="navigateToProfile()">Go to Parent's child</button>
						<router-outlet></router-outlet>
					   `,
					  imports: [RouterOutlet]
					})


					export class Parent { 
					  private router = inject(Router);
					  private route = inject(ActivatedRoute);

					  navigateToProfile() {
						this.router.navigate(['edit'],{relativeTo:this.route});
					  }
					}
				// here first of all we see the main content which is 'Go to Parent' on this click we redirct to /sujal and render parent componanent.
				// in the parent componanent we have to see the 'Go to Child' button on click to redicte the  /sujal childer edit componanent child and child is render in the parent <route-outlet>.
				// it's all parent to child rendering by helping ActivatedRoute and relativeTo.
			
			
		○ router.navigateByUrl() :
			- under this point we can direct make the url by written the string into the navigateByUrl('string here or url here').
			- parent child url are accept querystring, matrix accept.
			Ex.
				// Standard route navigation
				router.navigateByUrl('/products');
				// Navigate to nested route
				router.navigateByUrl('/products/featured');
				// Complete URL with parameters and fragment
				router.navigateByUrl('/products/123?view=details#reviews');
				// Navigate with query parameters
				router.navigateByUrl('/search?category=books&sortBy=price');
				// With matrix parameters
				router.navigateByUrl('/sales-awesome;isOffer=true;showModal=false')
				
			navigate() - pass the url as a array segments or parts. 
				ex. navigate(['/users', 10])
			
			navigateByUrl() - pass the url direct as a string.
				Ex. navigateByUrl('/users/10')
			// main thing both are consider the Router inject.
			

○ Read route state :
	- Angular Router allows you to read and use information associated with a route to create responsive and context-aware components.
	- read the querystring, url dynamical data etc read from componanent side.
	
	○ Get information about the current route with ActivatedRoute :
		- ActivatedRoute is a service from @angular/router that provides all the information associated with the current route.
		- it's give the all about the componanent information.
		Ex.
			import { Component } from '@angular/core';
			import { ActivatedRoute } from '@angular/router';
			@Component({
			  selector: 'app-product',
			})
			export class ProductComponent {
			  private activatedRoute = inject(ActivatedRoute); // important
			  constructor() {
				console.log(this.activatedRoute); // it's give the information componanent
			  }
			}
			
			The ActivatedRoute can provide different information about the route. Some common properties include:

			Property	Details
			url--------> An Observable of the route paths, represented as an array of strings for each part of the route path.
			data-------> An Observable that contains the data object provided for the route. Also contains any resolved values from the resolve guard.
			params-----> An Observable that contains the required and optional parameters specific to the route.
			queryParams-> An Observable that contains the query parameters available to all routes.
			
		○ Understanding route snapshots :
			- reading the query string, Route params, Fragment, Full static URL segments,  Route data.
			- it's done by ActivatedRoute and it's properties.
			
			Ex.
					route :
						  {
							path :'child/:id',
							component : Parent
						  }
					
					nav.html :
						<a [routerLink]="['child',205]">Go to</a>
					
					parent componanentn :	
						export class UserDetailComponent {

						  private route = inject(ActivatedRoute);
						  constructor() {
							// Route params (e.g., /users/101 → 101)
							console.log('User Id:', this.route.snapshot.params['id']);
							
							// Query params (?role=admin)
							console.log('Role:', this.route.snapshot.queryParams['role']);
							
							// Fragment (#profile)
							console.log('Fragment:', this.route.snapshot.fragment);
							
							// Full static URL segments
							console.log('URL:', this.route.snapshot.url);
							
							// Route data
							console.log('Data object:', this.route.snapshot.data);
						  }
						}

------------
15/12/2025
----------
○ Reading parameters on a route :
	○ Route Parameters :
		- Route parameters allow you to pass data to a component through the URL. This is useful when you want to display specific content based on an identifier in the URL, like a user ID or a product ID.
		- You can define route parameters by prefixing the parameter name with a colon (:).
		- here we can consider the reading the parameter of the URL.
		
		Ex.
			Route :
				  {
					path :'child/:id',
					component : Parent,
					
				  }
				  
			Nav.html :				
				<a [routerLink]="['child',2025]">Click Here</a>
				<router-outlet></router-outlet>
			
			Parent.ts :
				@Injectable({providedIn : 'root'})
				@Component({
				  selector: 'app-parent',
				  template: `
					<h2>Parent Component</h2>
					<h1>Product Details: {{ productId() }}</h1>
				   `,
				  imports: []
				})


				export class Parent { 
				  productId = signal('');
				  private activatedRoute = inject(ActivatedRoute); // Here ActivatedRoute is the used for the taking information about the URL.

				  constructor(){
					this.activatedRoute.params.subscribe((i)=>{
					  this.productId.set(i['id']);
					})
				  }
				  // here subscribe is the used for when url parameter value is change then fetch again from url.
				}
				
	○ Query Parameters :
		 - Query parameters provide a flexible way to pass optional data through URLs without affecting the route structure. Unlike route parameters, query parameters can persist across navigation events and are perfect for handling filtering, sorting, pagination, and other stateful UI elements.
		 - it's like a /child?id=191&model=2025&color=black so it's a fetched by the queryparams property of the ActivatedRoute.
		 Ex.(simple Example)
		 /profile?name=Sujal&age=22 // passed this
		 
		 home.ts :
			@Component({
			  selector: 'app-home',
			  template: `
				<button (click)="go()">Go with Query</button>
			  `
			})
			export class HomeComponent {
			  private router = inject(Router);

			  go() {
				this.router.navigate(['/profile'], {
				  queryParams: {
					name: 'Sujal',
					age: 22
				  }
				});
			  }
			}
		
			route.ts :
				export const routes = [
				  { path: 'profile', component: ProfileComponent }
				];

			profile.ts :
				@Component({
				  selector: 'app-profile',
				  template: `
					<h2>Profile Page</h2>
					<p>Name : {{ name }}</p>
					<p>Age : {{ age }}</p>
				  `
				})
				export class ProfileComponent {
				  private route = inject(ActivatedRoute); // reference for current url

				  name = ''; // empty string now
				  age = 0; // 0 value now

				  constructor() {
					this.route.queryParams.subscribe(q => { 
					  this.name = q['name'];
					  this.age = q['age'];
					});
				  }
				  // here fetch the url querystring value by queryparams with subscribe
				  // what happen here : when use redirect to this url with querystring then those querystring value is fetched here and show in the html page.
				}
				
		Ex(Major Example)
		- here make one select element there when the user select select other option then querystring is change with no refreshing page and chnage one key other key-value is as it is.
		- and one option is Add Page that is increment the page querystring key 1 to so on and also here page named key is updatable other is as it is.
		
		route.ts :
			{
				path: 'child',
				component: Parent
			}
			
		Nav.ts :
			export class Navv {
				item = inject(Router);
				qs(){
				this.item.navigate(['/child'],{
				  queryParams:{ // querystring pass here
					category:'Fast-Food',
					menuitem:'Name',
					page:1
				  }
				})
				}
				}

		Nav.html :
			<button (click)="qs()">Go to Querystring</button>
			// when button is clicked the pass the URL with querystring.
			<router-outlet></router-outlet>
			
		Parent.ts :
			@Component({
			  selector: 'app-parent',
			  template: `
				<h2>Product List</h2>
				<div>// first time passed querystring value is see here
				  <p>category : {{category}}</p>
				  <p>MenuItem : {{menuitem}}</p>
				  <p>Page : {{page}}</p>
				</div> 

				<select (change)="Changeopt($event)"> // when the selected option is changed then run the function Changeopt
				  <option value="">Select</option>
				  <option value="name">Name</option>
				  <option value="price">Price</option>
				  <option value="date">Date</option>
				</select>
				<hr>
				<button (click)="newPage()">Next Page</button> // call function when button is click
				<button (click)="this.item.navigateByUrl('')">Back</button>
				
			   `,
			  imports: []
			})


			export class Parent { 
			  private activatedRoute = inject(ActivatedRoute);
			  item = inject(Router)

			  category = '';
			  menuitem = '';
			  page= 1;

			  constructor(){
				  this.activatedRoute.queryParams.subscribe((i)=>{
				  // here get the querystring value by it's value.
				  this.category = i['category']
				  this.menuitem = i['menuitem'] || 'all';
				  this.page = Number(i['page']) || 1;
				  this.loadprint() // call the load function
				}) 
			  }

				// this function is call when the change the optional
			  Changeopt(event:Event){
					// in this line get the changed option value
				  const menuitem = (event.target as HTMLSelectElement).value;
				  this.item.navigate([],{
					// assign new option value to querystring key
					queryParams:{menuitem},
					// and merge the querystring with other key just this key is change
					queryParamsHandling:'merge'
				  })
			  }

				// updated option is console for testing
			  loadprint(){
				console.log(`Category : ${this.category} \nMenuitems :${this.menuitem} \nPage :${this.page} `);
			  }
			  
			  // when click new page button then run this function
			  newPage(){
				this.item.navigate([],{
				// assign the page key to new value with (+1)
				  queryParams:{page:this.page+1},
				  // same key merge with other querystring keys.
				  queryParamsHandling:'merge'
				})
			  }
			}
			
	
	○ Matrix Parameters :
		- Matrix parameters are optional parameters that belong to a specific URL segment, rather than applying to the entire route. Unlike query parameters which appear after a ? and apply globally, matrix parameters use semicolons (;) and are scoped to individual path segments.
		-Matrix parameters are useful when you need to pass auxiliary data to a specific route segment without affecting the route definition or matching behavior. Like query parameters, they don't need to be defined in your route configuration.

		Ex.
			// URL format: /path;key=value
			// Multiple parameters: /path;key1=value1;key2=value2
			// Navigate with matrix parameters
			this.router.navigate(['/awesome-products', { view: 'grid', filter: 'new' }]);
			// Results in URL: /awesome-products;view=grid;filter=new
		
	○ Detect active current route with RouterLinkActive :
		- in this concept when the user click the link or anchor tag that time attribute name 'routerLinkActive' has class apply on current clickable link.
		- we give to the class name to 'routerLinkActive' when the use click the link then the given class is apply on this link or anchor tag.
		
		Ex.			
			<nav>
			  <a routerLink="/child" routerLinkActive="active-link">Home</a>
			  <a routerLink="/sujal" routerLinkActive="active-link">About</a>
			</nav>
		
			.active-link {
				  background-color: red;
				  color: white;
				  text-decoration: none;
				  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
				}
			// when use click the any link then define class is apply on the link.
			
			
			<!-- Space-separated string syntax -->
			// here we give the multiple class by seprated space
			<a routerLink="/user/bob" routerLinkActive="class1 class2">Bob</a>
			
			<!-- Array syntax -->
			// here same give the multiple class by using array.
			<a routerLink="/user/bob" [routerLinkActive]="['class1', 'class2']">Bob</a>
			
		○ Route matching strategy :
			- given own exact url and user hit those url so apply given class but user not hit exact url so not apply classes on the current link.
			Ex.
				
				<a [routerLink]="['/user/jane']" routerLinkActive="active-link">
				  User
				</a>
				
				<a [routerLink]="['/user/jane/role/admin']" routerLinkActive="active-link">
				  Role
				</a>
			// here when the user go to the /user/jane then apply the active-link class apply and if user gone /user/jane/role/admin then also apply active-link.
			// on /user/jane apply active-link class because it's a part of the /user/jane/role/admin url.
		
		○ Only apply RouterLinkActive on exact route matches :
			- in above section not apply better class because some url is a subset of other urls so apply multiple class on multiple time so use the 'routerLinkActiveOptions'.
			- it's a used when exact matching concept apply.
			
			Ex.	
			<nav>
			  <a [routerLink]="['/sujal']" routerLinkActive="active-link"   [routerLinkActiveOptions]="{exact: true}">Home</a>
			  <a [routerLink]="['/sujal/edit']" routerLinkActive="active-link"   [routerLinkActiveOptions]="{exact: true}">About</a>
			</nav>
			// here user exact go to the /sujal then apply active-link otherwise not apply and also /sujal/edit goto then apply active-link by usign the exact:true.
			
		○ Apply RouterLinkActive to an ancestor :
			- in this example if the any of the anchor is exact then apply class on the div.
			
			Ex.
				<div routerLinkActive="active-link"   [routerLinkActiveOptions]="{exact: true}">
					<a routerLink="/sujal" >Home</a>
					<a routerLink="/sujal/edit">About</a>
				</div>
			
	○ Redirecting Routes :
		- Route redirects allow you to automatically navigate users from one route to another. Think of it like mail forwarding, where mail intended for one address is sent to a different address. This is useful for handling legacy URLs, implementing default routes, or managing access control.
		
		○ How to configure redirects :
			- You can define redirects in your route configuration with the redirectTo property. This property accepts a string.

			Ex.
				import { Routes } from '@angular/router';
				const routes: Routes = [
				  // Simple redirect
				  { path: 'marketing', redirectTo: 'newsletter' },
				  // Redirect with path parameters
				  { path: 'legacy-user/:id', redirectTo: 'users/:id' },
				  // Redirect any other URLs that don’t match
				  // (also known as a "wildcard" redirect)
				  { path: '**', redirectTo: '/login' }
				];
		
			In this example, there are three redirects:

				- When a user visits the /marketing path, they are redirected to /newsletter.
				- When a user visits any /legacy-user/:id path, they are routed to the corresponding /users/:id path.
				- When a user visit any path that’s not defined in the router, they are redirected to the login page because of the ** wildcard path definition.
				
		○ Understanding pathMatch :
			
			Value	    Description
			==================================================
			'full'	    The entire URL path must match exactly
			'prefix'	Only the beginning of the URL needs to match
			
		○ pathMatch: 'prefix' :
			- in this topic check the starting url like in this '/sujal/std/blog' so it's check the onlt start /sujal.
			Ex.
				export const routes: Routes = [
				  // This redirect route is equivalent to…
				  { path: 'news', redirectTo: 'blog },
				  // This explicitly defined route redirect pathMatch
				  { path: 'news', redirectTo: 'blog', pathMatch: 'prefix' },
				];
					
				/news redirects to /blog
				/news/article redirects to /blog/article
				/news/article/:id redirects to /blog/article/:id
				
		○ pathMatch: 'full' :
			- in this topic check the full path after working can gone next step.
			Ex.
				export const routes: Routes = [
				  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
				];
				// in this link check between '' all url and then take action.
			
			
		○ Conditional redirects :
			- The redirectTo property can also accept a function in order to add logic to how users are redirected.
			- The function only has access part of the ActivatedRouteSnapshot data since some data is not accurately known at the route matching phase. Examples include: resolved titles, lazy loaded components, etc.
			- It typically returns a string or URLTree, but it can also return an observable or promise.
			
			Ex.
				Route.ts :
					export const routes: Routes = [
					  {
						path: 'sujal/:lo',
						component: Parent
					  },
					  {
						path :'child/:id',
						component : ChildComponent,
						
					  },
					  {
						path:'sujal',
						redirectTo : (activatedRouteSnapshot)=>{
						// here get the querystring value
						  const par = activatedRouteSnapshot.queryParams['sub'];
						  const tim = new Date().getHours();
							
						// here check the querystring value is equal to Fd then redirct to given path other wise redirct to sujal/2955 path or url
						  if(par === 'Fd'){
							return `child/${tim}`
						  }
						  return `sujal/2955`
						}
					  }
					];
					
				Nav.html :
					<button (click)="qs()">Go to Querystring</button>
				
				Nav.ts :
					export class Navv {
					  item = inject(Router);
					  qs(){
					   this.item.navigate(['sujal'],{
						queryParams:{
						  sub:'Fd'
						}
					   })
					  }
				}

------------
16/12/2025
----------		
	○ Control route access with guards :
		- Route guards are functions that control whether a user can navigate to or leave a particular route. They are like checkpoints that manage whether a user can access specific routes. Common examples of using route guards include authentication and access control.
		
		○ Creating a route guard :
			- You can generate a route guard using the Angular CLI:
				ng generate guard CUSTOM_NAME(guard name)
			
			◘ TIP: You can also create a route guard manually by creating a separate TypeScript file in your Angular project. Developers typically add a suffix of -guard.ts in the filename to distinguish it from other files.	
			
		○ Route guard return types :
			Return types	Description
			boolean			true allows navigation, false blocks it (see note for CanMatch route guard)
			
			UrlTree or  	Redirects to another route instead of blocking
			RedirectCommand	
			
			Promise<T> or 	Router uses the first emitted value and then unsubscribes
			Observable<T>	
			
		○ Types of route guards :
			◘ CanActivate
			◘ CanActivateChild
			◘ CanDeactivate
			◘ CanMatch
		
			○ CanActivate :
				- in this guard used when the user hit inner url but user not login so this guard is check the user are login or not if it is then access the inner url but if not then it's redirect to login page.
				- so inshort that is the like a checkpoint which is check all user for login or not.
				
				Ex.
					Guard :
						
						export const logincheckGuard: CanActivateFn = (route, state) => {
						  debugger  
						  const rout = inject(Router);

						   rout.navigate(['/login']);
							return false;
						};
						
					Route File :
						export const routes: Routes = [
						  { 
							path:'',
							redirectTo:'login',
							pathMatch:'full'
						  },
						  {
							path:'login',
							component:Ulogin
						  },
						  {
							// access this route before apply logincheckGuard Guard logic
							path: 'protect',
							component: Parent,
							canActivate:[logincheckGuard] 
						  }
						];
					// what happen here : when user access this /protect url then apply Guard login to redirect /login url.
				
			○ CanActivateChild :
				- This Guard is helpfull for all child componanent protection.
				- apply single guard logic on all child componanent url.
				Ex.
					here we can consider the Guard logic is same as a canActivate guard logic.
				
				Route File :
					{
						path:'login',
						component:Ulogin,
						canActivateChild:[logincheckGuard],
						children :[
						  {path:'protect1',component:ChildComponent},
						  {path:'protect2',component:Ulogin}
						]
					  }
					// login route has 2 child so we can apply one single time guard and apply both children, so user hit the url protect1 & 2 then apply guard logic on this 2 child.
					
			○ CanDeactivate :
				- suppose we can fill the one long form and incase we can click the close or back button then page redirect to back page but problem is our form data is lost.
				- For this problem solution is CanDeactivate is used below condition incase click the back or close button then one conform popup is happen then asking the are you sure close the button or not.
				Ex.
					Ulogin.ts :
						@Component({
						  selector: 'app-ulogin',
						  template: `
							<h2>Login Page</h2>
							<form>
						  <div style="margin: 4%;">
							<label for="uname">Username :</label>
							<input type="text" placeholder="Username" name="uname" id="uname">
						  </div>
						  <div style="margin: 4%;">
							<button type="button">Login Now !</button>
						  </div>
						</form>
						  `
						})
						
						export class Ulogin {
						  private hasUnsaved = true;
						  
						  // deactivate function is here declare.
						  canDeactivate(): boolean {
							if (this.hasUnsaved) {
							  return confirm("Are You Sure Leave The Page ?");
							}
							return true;
						  }

						  savechanges() {
							this.hasUnsaved = false;
						  }

						}
					
					Guard File :
						export const deactivegGuard: CanDeactivateFn<unknown> = (component,currentRoute,currentstate, nextstate) => {
						// here when the click back button then check componanent canDeactivate function is available or not so it's available then it's call.
						  if(component && typeof(component as any).canDeactivate == 'function'){
							// canDeactivate function is available then return now
							return (component as any).canDeactivate();
						  }
						  // if not available then not do anything
						  return true;
						};
						
						==============================================================
						component: T - The component instance being deactivated
						currentRoute: ActivatedRouteSnapshot - Contains information about the current route
						currentState: RouterStateSnapshot - Contains the current router state
						nextState: RouterStateSnapshot - Contains the next router state being navigated to
						==============================================================
						
					Route File :
						export const routes: Routes = [
						  { 
							path:'',
							component:Navv
						  },
						  { 
						  // apply deactivegGuard guard on this componanent
							path:'Login',
							component:Ulogin,
							canDeactivate:[deactivegGuard]
						  },
						  {
							path:'Home',
							component:Parent
						  }
						];

				○ CanMatch :
					- CanMatch is the used when the some unauthorised user occur then componanent is not load on the browser.
					- suppose one Employee want to access the Admin Or Manager Url that time CanMatch can not load the admin or manager componanent on the browser, admin or manager Url is only read only for unauthorised user.
					- so role base authentication is follow this guard.
					
					EX.
						nav.ts :
							import { Component} from '@angular/core';
							import { RouterLink, RouterOutlet } from '@angular/router';

							@Component({
							  selector: 'app-nav',
							  imports: [RouterOutlet, RouterLink],
							  templateUrl: './nav.html',
							  styleUrl: './nav.css',
							})

							export class Navv {
							// declare function to save the localstorage for user role.
							  setRole(Role :string){
								console.log("Set Done !");
								localStorage.setItem('user',Role);
							  }
							}
							
						nav.html :
						// here click the button and set the user rolw as a admin or manager
							<button (click)="setRole('Admin')">As Admin</button>
							<hr>
							<button (click)="setRole('Manager')">As Manager</button>
							<hr>
							<a routerLink="Admin">Admin</a> <hr>
							<a routerLink="Manager">Manager</a>
							<router-outlet></router-outlet>
							
						rolebasedguard.ts :
							import {  CanMatchFn } from '@angular/router';
							export const roleBaseGuard: CanMatchFn = (route, state) => {
							
							// here match the user role from the localStorage for admin,manager
							  const userrole = localStorage.getItem('user');
							  
							// here check the path for admin and userrole value
							  if(route.path === 'Admin' && userrole === 'Admin'){
								return true;
							  }else if(route.path === 'Manager' && userrole === 'Manager'){
								return true;
								// if user has no specific role then it's only read only.
							  }else{
								return false;
							  }
							};

						route File :	
							export const routes: Routes = [
							  { 
								path:'',
								component:Navv
							  },
							  // apply on Ulogin page
							  { 
								path:'Admin',
								component:Ulogin,
								canMatch : [roleBaseGuard]
							  },
							  // also apply the guard here
							  {
								path:'Manager',
								loadComponent:()=>import('./parent/parent').then(r => r.Parent),
								canMatch : [roleBaseGuard]
							  }
							];
						
		○ Data resolvers :
			- Data resolvers allow you to fetch data before navigating to a route, ensuring that your components receive the data they need before rendering. This can help prevent the need for loading states and improve the user experience by pre-loading essential data.
			- Before Rendering the Url resolvers are get the data from API and that after render componanent.
			
			○ What are data resolvers?
				- A data resolver is a service that implements the ResolveFn function. It runs before a route activates and can fetch data from APIs, databases, or other sources. The resolved data becomes available to the component through the ActivatedRoute.
			
			○ Why use data resolvers?
				- Prevent empty states: Components receive data immediately upon loading
				- Better user experience: No loading spinners for critical data
				- Error handling: Handle data fetching errors before navigation
				- Data consistency: Ensure required data is available before rendering which is important for SSR
			
		○ Creating a resolver :
			Ex.
				resolver File :
					export const userResolver: ResolveFn<any> = (route,state) => {
					// here fetch the record from the api by user id.
						// user/:id  get id from this url
						 const parentid = route.paramMap.get('id');
						 // fetch the API by id
						 return fetch(`https://jsonplaceholder.typicode.com/todos/${parentid}`).then(res=>res.json());
						};
				
				Route File :
					 {
						path:'user/:id',
						component : Parent,
						// apply resolver and take the value to username variable
						resolve:{
						  username:userResolver
						}
					  }
				
				parent.ts :					
					@Component({
					  selector: 'app-parent',
					  template: `
						  <h2>Dashboard Parent</h2>   
							<p style="color:red">❌ Data load થવામાં error આવ્યો</p>
							// display API data
							<h4>User Id : {{ user.id }}</h4>
							<h4>Title : {{ user.title }}</h4>
						  <router-outlet></router-outlet>
					   `,
					  imports: [RouterOutlet]
					})


					export class Parent { 
					// takebale variable API value
					  user:any;
					  // ActivatedRoute for get the current url information
					  route = inject(ActivatedRoute);

					  ngOnInit(){
					  // put the fetched data to variable
						  this.user = this.route.snapshot.data['username'] ; 
					  }
					}
					
				nav.html :
					<a [routerLink]="['user','51']">Parent</a> <hr>
					
			○ Using withComponentInputBinding :
				- using this we can give to the API value to @Input() to direct into component.
				Ex.
					main.ts :
						bootstrapApplication(App, {
						  providers: [
						  // withComponentInputBinding is magical word to give the value direct componanent
							provideRouter(routes, withComponentInputBinding())
						  ]
						});
						
					// change the code to componanent where all API data is display
					// put the data into @Input() direct to componanent
					Parent.ts :
						export class Parent { 
						  route = inject(ActivatedRoute);
						  @Input() user : any; // here we can give the api value to direct @Input() instead of user:any;
						  ngOnInit(){
							  this.user = this.route.snapshot.data['username'] ; 
						  }
						}
						
			○ Error handling in resolvers :
				Resolver File :
					import { ResolveFn, Router } from '@angular/router';
					import { inject } from '@angular/core';

					export const userResolver: ResolveFn<any> = async (route, state) => {
					  let parentid = route.paramMap.get('id');
					  const router = inject(Router);
					  try{
						const res=  await fetch(`https://jsonplaceholder.typicode.com/todos/${parentid}`);
						// check the API throw response or not
						if(!res.ok){
						  throw new Error("User Not Found !");
						}
						// if throw responce then return to componanent 
						return res.json();
						// handle error
					  }catch(error){
						console.log(error);
						router.navigate(['/err'],{
						  queryParams:{parentid : parentid},
						  queryParamsHandling:'merge'
						});
						return false;
					  }
					};
					
				// all other file is same as above example.
				
-------------
17/12/2025
----------
		○ Managing errors through a subscription to router events :
			- You can also handle resolver errors by subscribing to router events and listening for NavigationError events. This approach gives you more granular control over error handling and allows you to implement custom error recovery logic.
			- inshort if any condition guard, resolver, laxy load will fail then NavigationError is handle this error on browser side.
			
			Ex.
			nav.html :
				<a [routerLink]="['user','50']">Parent</a> <hr>

			Resolver File :
				export const userResolver: ResolveFn<any> = async (route, state) => {
					const pid = route.paramMap.get('id');
					const req = await fetch(`https://jsonplaceholder.typicode.com/todos/${pid}`);
					if(!req.ok){
					  throw new Error("User Not Valid !");
					}else{
					  return req.json();
					}
				};
				// here if the API Response Is Not satifield And API Will Down by it's parameter then throw the error otherwise is fetch the Data from API.
				
			Routes File :
				{
					path:'user/:id',
					component : Parent,
					resolve:{data:userResolver}
				}
				// apply the resolver here
				
			Parent.ts :
				@Component({
				  selector: 'app-parent',
				  template: `
					  <h2>Dashboard Parent</h2>  
					  <router-outlet></router-outlet>
				   `,
				  imports: [RouterOutlet]
				})
				// if the api is fetched then show only <h2> tag.
				
			App.ts :
				
				export class App {
				  constructor(private router: Router) {
					this.router.events.subscribe(event => {
					  if (event instanceof NavigationError) {
						console.log('Resolver Error Detected');
						this.router.navigate(['/err']);
					  }
					});
				  }
				}
				// here apply the NavigationError on app componanent.
				// because the if the api is down then next componanent not loaded here if api is down then parent componanent is not render or loaded on browser so write the code of NavigationError On App.ts.
				
				// NavigationError -> it's help to when resolver or guard is fail then it's fired.
				
		○ Handling errors directly in the resolver :
			- in this topic handle the error on direct resolver.
			
			Ex.
				import { inject } from '@angular/core';
				import { ResolveFn, Router } from '@angular/router';

				export const userResolver: ResolveFn<any> = async (route) => {

				  const router = inject(Router);
				  const id = route.paramMap.get('id');

				  try {
					const res = await fetch(
					  `https://jsonplaceholder.typicode.com/todos/${id}`
					);

					if (!res.ok) {
					  throw new Error('User Not Found !');
					}
					return await res.json();

				  } catch (error) {
					console.error('Resolver handled error:');
					return { error: true };
				  }
				};// use the try & catch to handle the erro on in the resolver
				
		○ Navigation loading considerations :
			- While data resolvers prevent loading states within components, they introduce a different UX consideration: navigation is blocked while resolvers execute. Users may experience delays between clicking a link and seeing the new route, especially with slow network requests.
			- in this topic when the user hit the request then during fetching data show the UI loader for use exeriance.
			
			Ex.
				nav.ts :
					constructor(private router: Router){
					  isNavigating = computed(() => !!this.router.currentNavigation());
					}
					
				nav.html :
					@if(isNavigating()){
					  <p>Loading........</p>
					}
				
				// here when the api is searching the data that dring time <p>Loading....</p> is visible and then redirect to next page.
				// when we write this loading.... before the result page.
				Ex.	
					searching area (available input or search button, apply here)(nav.html)
						   |
					search result page(parent.html, it's a result page)
		
		○ Reading parent resolved data in child resolvers :
			- by inject ActivatedRoute and it's property snapshot.data[resolver name] to reading the resolver data.
			
			Ex.
				resolver File(datatest, resolver name) :	
					it's return 'sujal' only.
					
				route file :
					{
						path:'user',
						component:Parent,
						resolve:{data:datatest}
					}
				
				Parent.ts :
					export class Parent{
						routes  = inject(ActivatedRoute);
						ngOnInit(){
							console.log(this.routes.snapshot.Data['data'])// route resolver variable
						}
					}
				// when we hit the user or go Parent then console is give the resolver value 'sujal'
				
	○ Router Lifecycle and Events :
		- in this topic when the url is hit and url are render component in this time duration some Events are activated this is the Router Lifecycle and it's Events.
		- here we consider the fully life cycle of the Router and it's a evetns.
		
		🧭 High Level Navigation Flow : (Top to Bottom)
			User click link
			   ↓
			NavigationStart
			   ↓
			Route match
			   ↓
			Guards
			   ↓
			Resolvers
			   ↓
			Component create
			   ↓
			NavigationEnd
			
		○ RouterLifeCycle Events :
		
		Events				Description
		========-=============-================-===============-================-=============
		NavigationStart		Occurs when navigation begins and contains the requested URL.
		
		RoutesRecognized	Occurs after the router determines which route matches the URL and contains the route state information.
		
		GuardsCheckStart	Begins the route guard phase. The router evaluates route guards like canActivate and canDeactivate.
		GuardsCheckEnd		Signals completion of guard evaluation. Contains the result (allowed/denied).
		ResolveStart		Begins the data resolution phase. Route resolvers start fetching data.
		ResolveEnd			Data resolution completes. All required data becomes available.
		NavigationEnd		Final event when navigation completes successfully. The router updates the URL.
		NavigationSkipped	Occurs when the router skips navigation (e.g., same URL navigation).
				
		○ Error Events :
						
			Event				Description
			NavigationCancel	Occurs when the router cancels navigation. Often due to a guard returning false.
			NavigationError		Occurs when navigation fails. Could be due to invalid routes or resolver errors.
		
		○ How to subscribe to router events :
			Ex.
			
			import { Component, inject, signal } from '@angular/core';
			import { Router, NavigationError, RouterOutlet, NavigationStart, NavigationEnd, NavigationCancel } from '@angular/router';

			@Component({
			  selector: 'app-root',
			  standalone: true,
			  imports: [RouterOutlet],
			  template: `
			  @if (loading()) {
				  <div class="loader">Loading...</div>
				}
				<router-outlet />
				@if (error()) {
				  <div class="error">{{ error() }}</div>
				}
			  `
			})

			export class App {
			  private router = inject(Router);

			  loading = signal(false);
			  error = signal('');

			  constructor() {
				this.router.events.subscribe(event => {

					// first time page hit the url
				  if (event instanceof NavigationStart) {
					this.loading.set(true);
					this.error.set('');
				  }
					
					// end of the url hitting
				  if (event instanceof NavigationEnd) {
					this.loading.set(false);
					console.log('Analytics: Page view', event.url);
				  }
					
					// if the guard return false then fire this
				  if (event instanceof NavigationCancel) {
					this.loading.set(false);
					console.warn('Navigation Cancelled');
				  }
					
					// if any error occur then this fired
				  if (event instanceof NavigationError) {
					this.loading.set(false);
					this.error.set('Navigation failed!');
					console.error(event.error);
				  }
				});
			  }
			}
			// here some example to subscribe the Event on the component.
		
		○ How to debug routing events :
			- Debugging router navigation issues can be challenging without visibility into the event sequence. Angular provides a built-in debugging feature that logs all router events to the console, helping you understand the navigation flow and identify where issues occur.
			- When you need to inspect a Router event sequence, you can enable logging for internal navigation events for debugging. You can configure this by passing a configuration option (withDebugTracing()) that enables detailed console logging of all routing events.
				Ex.
					import {provideRouter, withDebugTracing} from '@angular/router';
						const appRoutes: Routes = [];
						bootstrapApplication(AppComponent, {
						  providers: [provideRouter(appRoutes, withDebugTracing())],
					});
		
		○ Common use cases :
			- Loading indicators
			- Analytics tracking
			- Error handling
	
	○ Testing routing and navigation :
		- pending !!!
		
		
-------------
18/12/2025
----------
	○ Other common Routing Tasks :
		○ Getting route information :
			- 1.Add withComponentInputBinding :
				- add this in the main.ts then access the route information by input in this condition not required the ActivateRoute Manually.
				- all ts file access the route information by using input not a ActivatedRoute.
				Ex.
					main.ts :
						bootstrapApplication(App,{
						  providers:[
							provideRouter(
							  routes,withComponentInputBinding()
						  )]
						})
						
					nav.html :
						<a [routerLink]="['/user', 1502]">Crisis Center</a>

					nav.ts :
						@Component({
						  selector: 'app-parent',
						  template: `
							  <h2>User Dashboard</h2>
							  <pre>{{ id() }}</pre>
							  <router-outlet></router-outlet>
						   `,
						  imports: [RouterOutlet]
						})

						export class Parent { 
						  id = input.required<string|undefined>();
						}
						
		○ Add an Input to the Component (with service) :
			- make the sevice to access the route infromation.
			Ex.
				product = computed(() =>
				  this.productService.getProductById(this.id())
				);
				
		○ Optional Parameter + Default Value :
			- 🔴 Problem
			
				URL sometimes:
				/products
				
				Sometimes:
				/products?id=10
				
				Solution :
					id = input.required({
					  transform: (value: string | undefined) => value ?? '1'
					});
					
					
					id = input<string | undefined>();
					finalId = linkedSignal(() => this.id() ?? '1');
					
					/products?id=5	5 // if id is available then access
					/products	1	// otherwise 1 is default
		
		○ Displaying 404 Page :
			- when router not declare and we can access it ,then throw a error so where wildcard route is helping.
			Ex.
				const routes: Routes = [
				  { path: 'products', component: ProductListComponent },
				  { path: '**', component: PageNotFoundComponent } // if the route not find then PageNotFoundComponent is render
				];
		
		○ Link Parameters Array (routerLink) :
			- 🔹 Simple Navigation :
				Ex.
					<a [routerLink]="['/products']">Products</a>
			
			- 🔹 Route Parameter :
				Ex.
					<a [routerLink]="['/products', 5]">View Product</a>
		
		○ Summary
			Topic						Meaning
			=========================   ====================
			withComponentInputBinding	Route → Component input
			input.required				Mandatory param
			transform					Default value
			**							404 page
			routerLink array			Smart navigation
			Child routes				Nested routing
			Hash strategy				No server config
		

○ Creating custom route matches :
	- The Angular Router supports a powerful matching strategy that you can use to help users navigate your application. This matching strategy supports static routes, variable routes with parameters, wildcard routes, and so on. Also, build your own custom pattern matching for situations in which the URLs are more complicated.
	
	○ Objectives
		- Implement Angular's UrlMatcher to create a custom route matcher.
		- here we can apply the custom logic to hit the URL from the user side and check the url parameter which is satifield for our custom router, and it's all happen by the UrlMatcher.
		
		Ex.
			- here we can check the url parameter is always star with the @ and not consider the '-'. like a /@sujal, /@angular.
			
			Ex.
				angular.json :
					- if this code is available in the json file so rapidlly Delete.
					 "server": "src/main.server.ts",
						"outputMode": "server",
						"ssr": {
						  "entry": "src/server.ts"
						}
						// beacause the this is affected in the custom router concept.
						
				main.ts :
					bootstrapApplication(App,{
					  providers:[
						provideRouter(
						  routes,withComponentInputBinding()
						)
					  ]
					}) // for the all parameter access by the @Input or input().
					
				routes file :
					import { Routes, UrlSegment } from '@angular/router';
					import { Parent } from './parent/parent';
					import { Navv } from './nav/nav';
					import { PageNotFound } from './NotFound/PageNotFound';


					export const routes: Routes = [
					  { 
						path:'',
						component:Navv
					  },
					  {
						path:'user/:id',
						component:Parent
					  },
					  {
					  // here make the custom router logic.
						matcher: (url) =>{ // url is the part of the UrlSegment	
							// in url consider only 1 segment like /@SUJAL 	not a /@user/@sujal
						  if(url.length === 1 && url[0].path.match(/^@[\w]+$/gm)){
							  return {
							consumed: url, // consume all full url
							posParams: { // here take the final path here consider the 0 or first world of the url like here '@' and it slice and dashboard is consume all final path and give to componanent
							dashboard: new UrlSegment(url[0].path.slice(1), {})}};
						  }
						  else{// if the route not matcher then it's return null and check next other routes
							  return null;
						  }
					  },// if url match then render Parent componanent
					  component: Parent
					  },
					  {
						path:'**',
						component:PageNotFound
					  }
					];

				Parent.ts:
					import { Component, Input } from "@angular/core";
					import { RouterOutlet} from "@angular/router";

					@Component({
					  selector: 'app-parent',
					  template: `
						  <h2>User Dashboard</h2>
						  <pre>{{ dashboard }}</pre>
						  <router-outlet></router-outlet>
					   `,
					  imports: [RouterOutlet]
					})

					export class Parent { 
					// routes dashboard url parameter is take and out in the @Input() because of withComponentInputBinding() in main.ts
					  @Input() dashboard!: string;
					}
					
		○ Rendering strategies in Angular :
			- What are rendering strategies?
				- Rendering strategies determine when and where your Angular application's HTML content is generated. Each strategy offers different trade-offs between initial page load performance, interactivity, SEO capabilities, and server resource usage.

				○ Angular supports three primary rendering strategies:
					○ Client-Side Rendering (CSR) - Content is rendered entirely in the browser
					○ Static Site Generation (SSG/Prerendering) - Content is pre-rendered at build time
					○ Server-Side Rendering (SSR) - Content is rendered on the server for the initial request for a route
		
				Ex.
					- if we want to content file like html,js,css are loaded first in the browser and after some time visible on the client side or browser then used the CSR.
					- if we want to content is static and no changes are apply on this file then used the SSG, it's immediate render on the browser like portfolio page, landing page etc.
					- if we want to display server side dynamical data to the html page then we used the SSR.
					
		
		○ Customizing route behavior :
		
			○ Handle canceled navigations :
				- if the navigation is cancel the angular has 2 option one is forcefully placed back url and one is placed natually to back url.
				canceledNavigationResolution: 'replace' | 'computed'
				
				replace is the forcefully placed the url and computed is natually placed.
				
			
			○ React to same-URL navigations :
				- in this topic when user reload the past reloaded url that time angular not consider because that is already hited so in this topic not happen like this.
				- if the hited url is again hited that time Guard, Api are not call again.
				Ex.
					onSameUrlNavigation: 'ignore' | 'reload'
				
				here ignore is the default behavior in result foe this ignore componanent not reload again, ngOnInit() not call again, Guard Not apply, Resolver not call again or API is not call again.
				- reload is the opposite work of the ignore.
				
			
			
			○ Control parameter inheritance :
				- when one parent has multiple child then url is make like /rootparent/child/grandchild so when we access the rootparent router paramaeter value to child or grandchild.
				- here use full the paramsInheritanceStrategy : 'always'.
				- we make without or with paramsInheritanceStrategy.
				
				Ex.
					routes file :
						{
							path: 'org/:orgId',
							component: Ulogin,
							children: [
							  {
								path: 'projects/:projectId',
								component: ChildComponent,
								children: [
								  {
									path: 'customers/:customerId',
									component: Parent,
								  },
								],
							  },
							],
						  }// here one Ulogin named Root parent, it's child is ChildComponent and it's  child is Parent.
						  - so here when we access Ulogin parameter value to Parent componanent so very critical but paramsInheritanceStrategy is use.
						
				Without paramsInheritanceStrategy :
				
					Parent.ts :
						import { Component, inject, Input } from "@angular/core";
						import { ActivatedRoute, RouterOutlet} from "@angular/router";

						@Component({
						  selector: 'app-parent',
						  template: `
							  <h2>User Dashboard</h2>
								<h4>Val0 : {{orgId}}</h4>
								<h4>Val1 : {{projectId}}</h4>
								<h4>Val2 : {{customerId }}</h4>
							  <router-outlet></router-outlet>
						   `,
						  imports: [RouterOutlet]
						})

						export class Parent { 
						  route = inject(ActivatedRoute);
							// without paramsInheritanceStrategy we call parent.parent.parent .... it's complex
						  orgId =this.route.parent?.parent?.snapshot.params['orgId'];
						  projectId = this.route.parent?.snapshot.params['projectId'];
						  customerId = this.route.snapshot.params['customerId'];

						  ngOnInit(){
							console.log(this.orgId);
							console.log(this.projectId);
							console.log(this.customerId);
						  }
						}
				With paramsInheritanceStrategy :
				
					main.ts :
						bootstrapApplication(App,{
						  providers:[
							provideRouter(
							// in this changes are make the easy wokr to access the params globally
							  routes,withRouterConfig({paramsInheritanceStrategy:'always'})
							)
						  ]
						})
					
					Parent.ts :	
						export class Parent { 
						  route = inject(ActivatedRoute);
							
							// access as a simple way direct by name
						  orgId =this.route.snapshot.params['orgId'];
						  projectId = this.route.snapshot.params['projectId'];
						  customerId = this.route.snapshot.params['customerId'];
						}
						
-------------
22/12/2025
----------
	○ Decide when the URL updates :
		- urlUpdateStrategy determines when Angular writes to the browser address bar. The default 'deferred' waits for a successful navigation before changing the URL. Use 'eager' to update immediately when navigation starts. Eager updates make it easier to surface the attempted URL if navigation fails due to guards or errors, but can briefly show an in-progress URL if you have long-running guards.
		- in this topic when the url is changed by own code that deside here.
		- by using urlUpdateStrategy with 'deferred' or 'eager' is help to do this proccess.
		- in this url hitting when the navigation is success then url is change otherwise not changed.
		- By deferred : when we hit the something url hiting in this time duration one guard is fired and check one condition and this hited url is not satifield for that condition then deferred is apply rapidally on the changing url.
		Ex.
			- one guard check the localstorage value check like islogin is available or not if this available then it's redirect to /org otherwise redirect to '/login'.
			- and other one make one routerlink '/org' and apply guard on '/org' so when we hit the '/org' then guard check islogin if available then redirect fastlly '/org' otherwise redirect direct '/login' and final url is /login.
			
			- same thing using the 'eager' if the guard is fail then first some micro second url is stay with '/org' and after some micro second is redirect to '/login'.
			- so deferred or eager is work deferent by asynclly one deferred is redirect directlly or eager is take some micro second after redirect to '/login'.
			
			Ex.
				logincheckGuard gurad :	
					import { inject } from '@angular/core';
					import { CanActivateFn, Router } from '@angular/router';


					export const logincheckGuard: CanActivateFn = async(route, state) => {
					  const router = inject(Router);
					  await new Promise(res=>setTimeout(res,3000));
					  const isLogin = localStorage.getItem('islogin');
					  
					  if (isLogin) {
						return true;   
					  }
					  return router.parseUrl('/login');
					};
				
				routerfile :
					{
						path: 'org',
						component: Ulogin,
						canActivate:[logincheckGuard]
					},
				
				Main.ts :
					bootstrapApplication(App,{
					  providers:[
						provideRouter(
						  routes,withRouterConfig({urlUpdateStrategy:'eager' & 'defered(default select)'})
						)
					  ]
					})
				
				Nav.html :
					<button (click)="Islo()">Login</button>
					<a [routerLink]="['org']">GO to</a>
				
				Nav.ts :
					export class Navv {
					  Islo(){
						localStorage.setItem('islogin',"sujalLogin");
					  }
					}
					
		○ Choose default query parameter handling :
			- in the queryParameter topic when the url is redirect then old new params are occur on the browser url when one url is hited with 2 paramaeter then new url is hitting then old paramaeter is remove and new paramaeter is apply.
			- using defaultQueryParamsHandling in the main.ts with 3 option replace(default),merge,preserve.
			- replace : new parameter replace old parameter.
			- merge : new parameter merge with old parameter.
			- preserve : if we want the old value then return old otherwise new.
			
			Ex.
				main.ts :
				bootstrapApplication(App,{
				  providers:[
					provideRouter(
					  routes,withComponentInputBinding(),withRouterConfig({defaultQueryParamsHandling:'replace & merge & preserve'})
					)
				  ]
				})
				
				nav.ts :
					@Component({
					  selector: 'app-nav',
					  imports: [RouterOutlet, RouterLink],
					  templateUrl: './nav.html',
					  styleUrl: './nav.css',
					})

					export class Navv {
					  item = inject(Router);

					  gotoorg(){
						this.item.navigate(['/org'])  
					  }

					  gotoorg2(){
						this.item.navigate(['/org'],{
						  queryParams:{december:"12thmonth"}
						})
					  }
					}
					
				nav.html :
					<button (click)="gotoorg()">Goto org</button>
					<button (click)="gotoorg()">Goto org 2</button>
	
	○ Route reuse strategy :
		- in the redirection method many componanent are import or destroy in between import or destroy much memory are busy so in this strategies are help to reuse the componanent after import one time means, if i import one componanent and angular can save it after other place we need to import those same componanent so multimple import is not satisfied for the angular app so there saved componanent instance is used.
		
		○ When to implement route reuse
			Custom route reuse strategies benefit applications that need:
			
				Form state preservation - Keep partially completed forms when users navigate away and return
				Expensive data retention - Avoid re-fetching large datasets or complex calculations
				Scroll position maintenance - Preserve scroll positions in long lists or infinite scroll implementations
				Tab-like interfaces - Maintain component state when switching between tabs
				
		○ Creating a custom route reuse strategy :
			- The RouteReuseStrategy class provides five methods that control the lifecycle of route components:

			Method			Description
			shouldDetach	Determines if a route should be stored for later reuse when navigating away
			store			Stores the detached route handle when shouldDetach returns true
			shouldAttach	Determines if a stored route should be reattached when navigating to it
			retrieve		Returns the previously stored route handle for reattachment
			
			shouldReuseRoute	Determines if the router should reuse the current route instance instead of destroying it during navigation
			
			shouldDetach = ask route the save or not
			store		 = if saved then store otherwise not
			shouldAttach = render time saved route can attach or create new instance
			retrieve     = saved instance return
			shouldReuseRoute = Routeconfig same use or not
			
			
			Ex.
				CustomRouteReuseStrategy.ts :
					import { ActivatedRouteSnapshot, DetachedRouteHandle, Route, RouteReuseStrategy } from "@angular/router";

					export class CustomRouteReuseStrategy  extends RouteReuseStrategy{
					  private handler = new Map<Route | null, DetachedRouteHandle>();
					 
					  // check the permission is true or false (true then save false then not save destroy)
					  override shouldDetach(route: ActivatedRouteSnapshot): boolean {
						  return route.data['reuse'] === true;
					  }

					  // on the shouldDetach decision if true then save in the map otherwise not
					  override store(route: ActivatedRouteSnapshot, handle: DetachedRouteHandle | null): void {
						  if(handle && route.data['reuse']=== true){
							const key= this.GetRouteKey(route);
							this.handler.set(key,handle);
						  } 
					  }

					  // when the user open the route then saved instance do reattached or new create
					  override shouldAttach(route: ActivatedRouteSnapshot): boolean {
						const key= this.GetRouteKey(route);
						return route.data['reuse'] === true && this.handler.has(key);
					  }

					  // actual instance return from the map
					  override retrieve(route: ActivatedRouteSnapshot): DetachedRouteHandle | null {
						const key= this.GetRouteKey(route);
						return route.data['reuse'] === true ?(this.handler.get(key) ?? null) : null;
					  }
					  
					  override shouldReuseRoute(future: ActivatedRouteSnapshot, curr: ActivatedRouteSnapshot): boolean {
						return future.routeConfig === curr.routeConfig;
					  }

					  private GetRouteKey(router : ActivatedRouteSnapshot):Route | null{
						return router.routeConfig;
					  }
					  
					}
					
				main Or appconfig file :
					    {provide: RouteReuseStrategy,useClass:CustomRouteReuseStrategy},

				route file :
					
					export const routes: Routes = [
					  { 
						path:'',
						component:Navv
					  },
					  {
						path: 'org',
						component:Ulogin,
						data:['reuse'] // reuse concept apply here so one time import and reuse anywhere
					  },
					  {
						path:'par',
						component:Parent // not apply reuse concept one time import and after movment destroy
					  },
					  {
						path:'**',
						component:PageNotFound
					  }
					];
			
			○ Preloading strategy :
				- when the follow simple way to load the componanent in the angular app there without Preloading strategies when we open /home then home load when we open /office then office componanent is load in between loader are in proccess and it's all about the waste of the time.
				- so Preloading is the help to Put the all imported componanent behind the backgorund for use for the redirection time.
				
				Angular provides two preloading strategies out of the box:

					Strategy		Description
					
					NoPreloading	The default strategy that disables all preloading. In other words, modules only load when users navigate to them
					
					PreloadAllModules	Loads all lazy-loaded modules immediately after the initial navigation
				 
				Ex.
					Make First Custom Preloading Strategies:
					
					Preloadingst.ts :
						
					import { PreloadingStrategy, Route } from "@angular/router";
					import { Observable, of } from "rxjs";

					export class Preloadingst implements PreloadingStrategy {

					  preload(route: Route, fn: () => Observable<any>): Observable<any> {
						if(route.data?.['preload']){
						  return fn();
						}
						return of(null);
					  }
					}
					
					router file :
						{
							path: 'org',
							loadComponent:()=>import("../ulogin/ulogin").then(r=>r.Ulogin),
							data:{preload:true} // preload apply
						  },
						  {
							path:'par',
							loadComponent:()=>import("./parent/parent").then(r=>r.Parent),
							data:{preload:false} // preload not apply   
						},
					
					Main.ts :
						bootstrapApplication(App,{
						  providers:[
							provideRouter(
							  routes,withPreloading(Preloadingst)
							)
						  ],
						})
						
	○ URL handling strategy :
		- under this concept angular deside the which type of the url are angular handle, suppose the start with the '/app' all url are handle by angular and other like '/or','/opp' handle by other tool or application.
		Ex.
			urlUpdateStrategy class :
				import { Injectable } from "@angular/core";
				import { UrlHandlingStrategy, UrlTree } from "@angular/router";

				Injectable()
				export class urlhandlingstrategy implements UrlHandlingStrategy{
				  
				  shouldProcessUrl(url: UrlTree): boolean {
				  // deside the which url is handle by angular
					return url.toString().startsWith('/org');
				  }
				  extract(url: UrlTree): UrlTree {
				  // if handle then return
					return url;
				  }
				  merge(newUrlPart: UrlTree, rawUrl: UrlTree): UrlTree {
					return newUrlPart;
				  }
				}
				
			appConfig.ts :
				    {provide: UrlHandlingStrategy(strategy class),useClass:urlhandlingstrategy(own made class)},
			

	○ Custom route matchers :
		- when the user hit the undefine route then application is return the error like path not match like this so we can make the '**' wildcard route to redirect not found page.
		- but when we create the custom matchers for whole application to apply custom matchers to wide application by using UrlMatchResult.
		- there we define own custom rule to apply on the whole application.
		
		Ex.
			Custom Url Matcher File :
				import { UrlMatchResult, UrlSegment } from "@angular/router";

				export function custommatching(segment:UrlSegment[]):UrlMatchResult|null{
				  if(segment.length === 1 && segment.toString().startsWith('user-')){
					return{
					  consumed:segment,
					  posParams:{
						id: new UrlSegment(segment[0].toString().split('user-')[1],{}),
					  }
					}
				  }
				  return null;
				}
				// here simple example to make some simple rule for hiting the url 
				// here when the user hit the 1 segment url like 1 segment = '/user','/test' that consider the matchers.
				// '/sujal/test','/test/org' is not a 1 segment is the 2 segment url.
				// also 1 segment is starwith 'user-' must be and it's return with consumed(consume the value from url),posParams(give to the parameter with value)
				// that parameter is fetched or access by the componanent by ActivatedRoute.
				// if the 'user-' or not affect this rule to other url then it's return null or nothing.
				
				(Easy)
			Router File :
				 {
					matcher:custommatching,
					component:Parent
				 },
			
			Parent.ts :
				@Component({
				  selector: 'app-parent',
				  template: `
					  <h1>Parent</h1>
					  <p>{{id}}</p>
					  <router-outlet></router-outlet>
				   `,
				  imports: [RouterOutlet]
				})

				export class Parent { 
				  id!:string;
				  constructor(private item : ActivatedRoute){
				  // access the value which is throw by the custom matcher
					item.params.subscribe(e=>{
					  this.id = e['id'];
					  console.log(e.toString());
					})
				  }
				}
				
			
			==== Some Businness Login Apply (Advanced)
					Custom Url Matcher File :
						import { UrlMatchResult, UrlSegment } from "@angular/router";
						export function custommatching(segment:UrlSegment[]):UrlMatchResult|null{
						  if(segment.length === 0) return null;
						  const fsegment = segment[0].path;

						  if(fsegment.startsWith('sujal-')){
							return {
							  consumed:[segment[0]],
							  posParams:{
								mainheading :new UrlSegment(fsegment.split('-')[0],{}),
								sujalneed:new UrlSegment(fsegment.split('-')[0].toUpperCase(),{}),
								sujalhasmoney: new UrlSegment('true',{})
							  }
							}
						  }

						  if(fsegment.startsWith('test-')){
							return {
							  consumed:[segment[0]],
							  posParams:{
								mainheading :new UrlSegment(fsegment.split('-')[0],{}),
								sujalneed:new UrlSegment(fsegment.split('-')[0].toUpperCase(),{}),
								sujalhasmoney: new UrlSegment('true',{})
							  }
							}
						  }

						  
						  return null;
						}
					
					Parent.ts :
						import { Component, inject, input, Input } from "@angular/core";
						import { ActivatedRoute, RouterOutlet} from "@angular/router";

						@Component({
						selector: 'app-parent',
						template: `
						  <h1>Parent</h1>
						  @if(is === 'true'){
							<h3>{{head}} fired</h3>
							<p>{{id}}</p>
							<p>{{is}}</p>
						  }
						  <router-outlet></router-outlet>
						`,
						imports: [RouterOutlet]
						})

						export class Parent { 
						id!:string;
						is!:string;
						head!:string;
						constructor(private item : ActivatedRoute){
						item.params.subscribe(e=>{
						  this.id = e['sujalneed'];
						  this.is = e['sujalhasmoney'];
						  this.head = e['mainheading'];
						  console.log(e.toString());
						})
						}
						}
					
					// here apply multiple url matchers with multiple value passed like '/test-','/sujal-' or passed 3 value at a time and display dynamically in the componanent.
					
		○ Router reference :
			- ○ Router events :
				
				Router event			Details
				NavigationStart			Triggered when navigation starts.
				RouteConfigLoadStart	Triggered before the Router lazy loads a route configuration.
				RouteConfigLoadEnd		Triggered after a route has been lazy loaded.
				RoutesRecognized		Triggered when the Router parses the URL and the routes are recognized.
				GuardsCheckStart		Triggered when the Router begins the Guards phase of routing.
				ChildActivationStart	Triggered when the Router begins activating a route's children.
				ActivationStart			Triggered when the Router begins activating a route.
				GuardsCheckEnd			Triggered when the Router finishes the Guards phase of routing successfully.
				ResolveStart			Triggered when the Router begins the Resolve phase of routing.
				ResolveEnd				Triggered when the Router finishes the Resolve phase of routing successfully.
				ChildActivationEnd		Triggered when the Router finishes activating a route's children.
				ActivationEnd			Triggered when the Router finishes activating a route.
				NavigationEnd			Triggered when navigation ends successfully.
				NavigationCancel		Triggered when navigation is canceled. This can happen when a Route Guard returns false during navigation, or redirects by returning a UrlTree or RedirectCommand.
				
				NavigationError			Triggered when navigation fails due to an unexpected error.
				Scroll					Represents a scrolling event.
		
			○ Router terminology :
				Router terminology

				Router part			Details
				Router				Displays the application component for the active URL. Manages navigation from one component to the next.
				
				provideRouter		provides the necessary service providers for navigating through application views.
				
				RouterModule		A separate NgModule that provides the necessary service providers and directives for navigating through application views.
				
				Routes				Defines an array of Routes, each mapping a URL path to a component.
				
				Route				Defines how the router should navigate to a component based on a URL pattern. Most routes consist of a path and a component type.
				
				RouterOutlet		The directive (<router-outlet>) that marks where the router displays a view.
				
				RouterLink			The directive for binding a clickable HTML element to a route. Clicking an element with a routerLink directive that's bound to a string or a link parameters array triggers a navigation.
				
				RouterLinkActive	The directive for adding/removing classes from an HTML element when an associated routerLink contained on or inside the element becomes active/inactive. It can also set the aria-current of an active link for better accessibility.
				
				ActivatedRoute		A service that's provided to each route component that contains route specific information such as route parameters, static data, resolve data, global query parameters, and the global fragment.
				
				RouterState			The current state of the router including a tree of the currently activated routes together with convenience methods for traversing the route tree.
				
				Link parameters array	An array that the router interprets as a routing instruction. You can bind that array to a RouterLink or pass the array as an argument to the Router.navigate method.
				
				Routing component	An Angular component with a RouterOutlet that displays views based on router navigations.
				
		○ <base href> :
			- in this topic we can declare the starting point of the application.
			- just apply in the index.html <base href> and declare the starting point of the application.
			
			Ex.
				<base href="/angular-app/">
				// above means the application starting by the /angular-app/.
			
		○ HashLocationStrategy :
			- Use HashLocationStrategy by providing the useHash: true in an object as the second argument of the RouterModule.forRoot() in the AppModule.
	
			Ex.
			
				providers: [provideRouter(appRoutes, withHashLocation())];
		
			When using RouterModule.forRoot, this is configured with the useHash: true in the second argument: RouterModule.forRoot(routes, {useHash: true}).
		
	○ Route transition animations :
		- In this topic when we perform the routing that time apply the animation like fade in and fade out etc that's called the Route transition animatio.
		
		Ex.
			Make First Animation conmoponent files :
			animation.ts :
				import { Component } from '@angular/core';
				import { RouterLink, RouterLinkActive, RouterOutlet } from '@angular/router';
				import { trigger, transition, style, animate, query, group } from '@angular/animations';

				@Component({
				  selector: 'app-animation',
				  imports: [RouterOutlet,RouterLink],
				  templateUrl: './animation.html',
				  styleUrl: './animation.css',
				  animations:[
				  // give the animation name here
					trigger('RouterAnimation',[
					// decalre the animation apply from start to and or any where
					  transition('* <=> *',[
					  // fire query for the animation
						query(':enter, :leave',[
						  style({
							position:'absolute',
							width:'100%',
							opacity:0
						  })
						  // option true is help to handle error
						],{optional:true}),

						// where the we wnter the page then applt this animation
						query(':enter',[
						  animate('500ms ease',style({opacity:1}))
						],{optional:true})

					  ])
					])
				  ]
				})
				export class Animation {
				
					// verified the animation by fetch the data of the route
				  prepare(outlet:RouterOutlet){
					return outlet.activatedRouteData?.['animation'];
				  }
				}
				
			animation.html :
				<nav>
				  <a routerLink="/Animation-Desk/Parent">Parent</a> |
				  <a routerLink="/Animation-Desk/Child">Child</a> |
				  <a routerLink="/Animation-Desk/ThirdParty">Other</a> 
				</nav>
				
				// apply here animation
				// give the animation attribute by it's name
				<div [@RouterAnimation]="prepare(outlet)">
				  <router-outlet #outlet="outlet"></router-outlet>
				</div>

○ Forms in Angular :
	- Applications use forms to enable users to log in, to update a profile, to enter sensitive information, and to perform many other data-entry tasks.
	- Angular provides two different approaches to handling user input through forms: reactive and template-driven.
	- Both capture user input events from the view, validate the user input, create a form model and data model to update, and provide a way to track changes.
	
	 ○ Choosing an approach :
		Reactive forms and template-driven forms process and manage form data differently. Each approach offers different advantages.

Forms					Details
Reactive forms:			Provide direct, explicit access to the underlying form's object model. Compared to template-driven forms, they are more robust: they're more scalable, reusable, and testable. If forms are a key part of your application, or you're already using reactive patterns for building your application, use reactive forms.

Template-driven forms:	Rely on directives in the template to create and manipulate the underlying object model. They are useful for adding a simple form to an app, such as an email list signup form. They're straightforward to add to an app, but they don't scale as well as reactive forms. If you have very basic form requirements and logic that can be managed solely in the template, template-driven forms could be a good fit.
				

	○ Key differences :
		
		The following table summarizes the key differences between reactive and template-driven forms.

									Reactive								Template-driven
			Setup of form model		Explicit, created in component class	Implicit, created by directives
			Data model				Structured and immutable				Unstructured and mutable
			Data flow				Synchronous								Asynchronous
			Form validation			Functions								Directives
				
	○ Common form foundation classes
		
		Both reactive and template-driven forms are built on the following base classes.

			Base classes			Details
			FormControl				Tracks the value and validation status of an individual form control.
			FormGroup				Tracks the same values and status for a collection of form controls.
			FormArray				Tracks the same values and status for an array of form controls.
			ControlValueAccessor	Creates a bridge between Angular FormControl instances and built-in DOM elements.

		○ Setup in reactive forms :
			-	import {Component} from '@angular/core';
				import {FormControl, ReactiveFormsModule} from '@angular/forms';
				@Component({
				  selector: 'app-reactive-favorite-color',
				  template: ` Favorite Color: <input type="text" [formControl]="favoriteColorControl" /> `,
				  imports: [ReactiveFormsModule], // import first ReactiveFormsModule
				})
				
				export class FavoriteColorReactiveComponent {
				  favoriteColorControl = new FormControl(''); // hadle single input by the FormControl
				} 	
				
				
		○ Setup in template-driven forms :
			import {Component} from '@angular/core';
			import {FormsModule} from '@angular/forms';
			@Component({
			  selector: 'app-template-favorite-color',
			  template: ` Favorite Color: <input type="text" [(ngModel)]="favoriteColor" /> `,
			  imports: [FormsModule], // import first FormsModule
			})
			export class FavoriteColorTemplateComponent {
			  favoriteColor = ''; // handle the single variable inputs
			}
			
		○ Mutability of the data model

			The change-tracking method plays a role in the efficiency of your application.

			○ Reactive forms :	Keep the data model pure by providing it as an immutable data structure. Each time a change is triggered on the data model, the FormControl instance returns a new data model rather than updating the existing data model. This gives you the ability to track unique changes to the data model through the control's observable. Change detection is more efficient because it only needs to update on unique changes. Because data updates follow reactive patterns, you can integrate with observable operators to transform data.
			
			○ Template-driven forms :	Rely on mutability with two-way data binding to update the data model in the component as changes are made in the template. Because there are no unique changes to track on the data model when using two-way data binding, change detection is less efficient at determining when updates are required.
						
		○ Form validation :
			
			Reactive forms :	Define custom validators as functions that receive a control to validate
			Template-driven forms :	Tied to template directives, and must provide custom validator directives that wrap validation functions
			
○ Reactive forms :
	- Reactive forms provide a model-driven approach to handling form inputs whose values change over time. This guide shows you how to create and update a basic form control, progress to using multiple controls in a group, validate form values, and create dynamic forms where you can add or remove controls at run time.
	
	Ex.
		Reactiveform.ts :
			import { CommonModule } from '@angular/common';
			import { Component, inject } from '@angular/core';
			import { AbstractControl, FormArray, FormBuilder, FormControl, FormGroup, FormRecord, isFormArray, isFormControl, isFormGroup, isFormRecord, ReactiveFormsModule, StatusChangeEvent, TouchedChangeEvent, Validators, ValueChangeEvent } from '@angular/forms';
			import { RouterLink } from '@angular/router';

			@Component({
			  selector: 'app-reactiveform',
			  standalone: true,
			  imports: [ReactiveFormsModule, RouterLink,CommonModule], // Imports Section
			  templateUrl: './reactiveform.html',
			  styleUrl: './reactiveform.css',
			})

			export class Reactiveform {
			  profileresult :any[] = []; // empty array

			  // using  formgroup formcontrol
			  profiledetail = new FormGroup({// collection of the multiple FormControl
				fname: new FormControl(''), // handle single input 
				lname: new FormControl(''),
				age : new FormControl(''),
				adress : new FormGroup({ // nested formgroup & FormControl
				  street: new FormControl(''),
				  landmark: new FormControl(''),
				  city: new FormControl(''),
				  zipcode: new FormControl('')
				})
			  })
       
			  dis(){ // function to add data from the html side to empty array
				this.profileresult.push(this.profiledetail.value);
				this.profiledetail.reset();
			  }

			  // using FormBuilder
			  // using this technique we can decalre the form as less world
			  profilebuilder: any[] = [];
			  private formbuilder = inject(FormBuilder); // inject first formbuilder
			  profilebybuilder = this.formbuilder.group({ // access group as a formgroup
				firstname:['',Validators.required], // apply validation here
				lastname:[''], // in simple way declare FormControl but here this style is apply
				adress: this.formbuilder.group({
				  city:[''],
				  zip:['']
				}),
				hobby : this.formbuilder.array([this.formbuilder.control('')]) // apply the formarray concept to add more or dynamic form
			  })
				
				// function for the formbuilder form 
			  disbybyuilder(){
				this.profilebuilder.push(this.profilebybuilder.value);
				this.profilebybuilder.reset(); // reset all form and form become empty
				// console.log(this.profilebuilder.values.toString)
			  }
				
				// get all hobby from the formbuilder form declarations by using formarray
			  get hobby(){
				return this.profilebybuilder.get('hobby') as FormArray;
			  }
				
				// runtime we can add element in the form
			  addhob(){
				return this.hobby.push(this.formbuilder.control(''));
			  }
				
				// Runtime we can remove the form element
			  removehob(index : number){
				return this.hobby.removeAt(index);
			  }

			  // form array directive
			  //  Sujal = new FormArray([
			  //   new FormControl('fish'),
			  //   new FormControl('cat'),
			  //   new FormControl('dog'),
			  // ]);

			  // form events
			  // handle formEvents
			  testing = new FormGroup({
				test: new FormControl(''),
			  })

			  constructor(){
				this.testing.events.subscribe((e)=>{// subscribe the formevents
					if(e instanceof ValueChangeEvent){ // when form element value change then fire this
					  console.log("Value Changed ", e.value)
					}
					if(e instanceof StatusChangeEvent){ // form status change then fired this
					  console.log("Status Chnaged ",e.status)
					}
					if(e instanceof TouchedChangeEvent){ // touched untouched then fired this
					  console.log("touch Untouch ",e.touched)
					}

				})
			  }

			  // Here is one complete, real Angular example showing ALL 4 utility functions
			  
			  form = new FormGroup({
				name: new FormControl('Sujal'),
				skills: new FormArray([
				  new FormControl('Angular'),
				  new FormControl('React'),
				]),
				settings: new FormRecord({
				  theme: new FormControl('dark'),
				  language: new FormControl('en')
				})
			  });

			  inspect(con : AbstractControl){
				if(isFormControl(con)){
				  console.log("Array Form : ",con.value); // fetch all formcontrol value here
				}
				if(isFormArray(con)){
				  console.log("Form array : ",con.length); // formarray value
				}
				if(isFormRecord(con)){
				  console.log("Form record : ",Object.keys(con.controls)); // formrecord here
				}
			  }


			  // type form
			  // without using the nonNullable when form is reset then formcontrol became the null
			  // but here using the nonNullable it's help to formcontrol for the reset not apply on this by using nonNullable
			  login = new FormGroup({
				// nonNullale Is help to not alllow to reset value in the form control
				email : new FormControl('test@gmail.com',{nonNullable:true}),
				psw: new FormControl(''),
			  });


			  username(){
				const email = this.login.value.email;
				this.login.reset();
				console.log(email); // if the nonNullable is not available then value is become null but here it is so not null
			  }

			  explittype(){
				const explititype = new FormControl<string | null>(null);
				explititype.setValue("Explicit Type")
				console.log(explititype.value);
			  }
			}
		
		reactiveforms.html :
			<h1>Reactive Form :</h1>
			<form [formGroup]="profiledetail" (ngSubmit)="dis()">
			  <div style="display: flex; flex-direction: column; gap: 20px;">

				<div>
				  <label for="">First Name : </label>
				  <input type="text" placeholder="Fname" formControlName="fname"> // using formcontrol name put here
				</div>
				<div>
				  <label for="">Last Name : </label>
				  <input type="text" placeholder="Lname" formControlName="lname">
				</div>
				<div>
				  <label for="">Age : </label>
				  <input type="number" placeholder="Age" formControlName="age">
				</div>

				<h3>Adress :</h3>
				<div formGroupName="adress" style="display: flex; flex-direction: column; gap: 20px;">
				  <div>
					<label for="">Street Name : </label>
					<input type="text" placeholder="Street Name" formControlName="street">
				  </div>
				  <div>
					<label for="">landmark : </label>
					<input type="text" placeholder="Landmark" formControlName="landmark">
				  </div>
				  <div>
					<label for="">City : </label>
					<input type="text" placeholder="City Name" formControlName="city">
				  </div>
				  <div>
					<label for="">Zipcode : </label>
					<input type="number" placeholder="Zipcode" formControlName="zipcode" min="6">
				  </div>
				</div>

				<div>
				  <button type="submit" [disabled]="!profiledetail.valid">Save</button>
				</div>

			  </div>
			</form>
			
			// fetch all value from the empty array
			<ul *ngFor="let data of profileresult">
			  <li>First Name : <strong>{{data.fname}}</strong></li>
			  <li>Last Name : <strong>{{data.lname}}</strong></li>
			  <li>Age : <strong>{{data.age}}</strong></li>
			  <li>Street Name : <strong>{{data.adress.street}}</strong></li>
			  <li>Landmark : <strong>{{data.adress.landmark}}</strong></li>
			  <li>City : <strong>{{data.adress.city}}</strong></li>
			  <li>ZipCode : <strong>{{data.adress.zipcode}}</strong></li>
			</ul>

			<hr>
			
			<h2>Using Form Builder :</h2>

			<form [formGroup]="profilebybuilder" (ngSubmit)="disbybyuilder()">
			  <div style="display: flex; flex-direction: column; gap: 20px;">

				<div>
				  <label for="">First Name : </label>
				  <input type="text" placeholder="Fname" formControlName="firstname">
				</div>
				<div>
				  <label for="">Last Name : </label>
				  <input type="text" placeholder="Lname" formControlName="lastname">
				</div>

				<h3>Adress :</h3> // apply here nested formgroup & formcontrol
				<div formGroupName="adress" style="display: flex; flex-direction: column; gap: 20px;">
				  <div>
					<label for="">City : </label>
					<input type="text" placeholder="City Name" formControlName="city">
				  </div>
				  <div>
					<label for="">Zipcode : </label>
					<input type="number" placeholder="Zipcode" formControlName="zip" min="6">
				  </div>
				</div>

				<div>
				  <button type="submit" [disabled]="!profilebybuilder.valid">Save</button>
				</div>
			  </div>


			  <h4>Form Array :</h4> // apply here runtime add formelement
			   <div  formArrayName="hobby" style="display: flex; flex-direction: column; gap: 10px;">
				 @for(hob of hobby.controls; track $index; let h = $index){ // apply the for loop to hobby named formArray
				   <div>//which type of we can add on the form that's add here style and html
					 <label for="hob-{{h}}">Hobby {{h+1}}: </label>
					 <input type="text" id="hob-{{h}}" [formControlName]="h">
					 @if(h >= 1){
					   <button (click)="removehob(h)">Delete</button>
					  }
					</div>
				  }
				  <button style="width: 10vw;" type="button" (click)="addhob()">Add Hobby</button>
			   </div>



			</form>

			<div>
			  <p>Form Status : {{profilebybuilder.status}}</p> // we know the form status
			</div>

			<ul *ngFor="let data of profilebuilder">
			  <li>First Name : <strong>{{data.firstname}}</strong></li>
			  <li>Last Name : <strong>{{data.lastname}}</strong></li>
			  <li>City : <strong>{{data.adress.city}}</strong></li>
			  <li>ZipCode : <strong>{{data.adress.zip}}</strong></li>
			  <li>Hobbies:</li>
			  <ul>
				<li *ngFor="let hob of data.hobby">
				  {{ hob }}
				</li>
			  </ul>
			</ul>

			<hr>
			<!-- <h2>Using FormArrayDirective</h2>
			   <form [formArrayName]="Sujal">
				  @for (control of Sujal.controls; track $index) {
					<input [formControlName]="$index">
				  }
				</form> -->
			<hr>

			<hr>
				<h2>Form Events</h2> // for this test the event
				<div [formGroup]="testing">
				  <input type="text" placeholder="Testing Events" formControlName="test">
				</div>
			<hr>


			<h2>Utility Functions Example(Events)</h2>

			<p><strong>Name:</strong> {{ form.value.name }}</p>

			<button (click)="inspect(form.get('name')!)">See Constrol</button>

			<button (click)="inspect(form.get('skills')!)">See Form Array</button>
			<button (click)="inspect(form.get('settings')!)">See Form Record</button>

			<h3>Skills:</h3>
			<ul>
			  <li *ngFor="let s of form.value.skills">{{ s }}</li>
			</ul>

			<hr>

			
			<form [formGroup]="login" (ngSubmit)="username()">
			  <div>
				<label for="email">Email : </label>
				<input type="text" id="email" formControlName="email">
			  </div>
			  <div>
				<label for="psw">Password : </label>
				<input type="text" id="psw" formControlName="psw">
			  </div>
			  <div>
				<button type="submit">Save</button>
			  </div>

			</form>

			<button (click)="explittype()">Explit Type</button>


			<hr>


			<div style="margin-top: 10px;">
			<a [routerLink]="['']" style="text-decoration: none; border: 2px solid black; padding: 0.5%; color: white; background-color: red;">Back</a>
			</div>


○ Building a template-driven form :
	- This tutorial shows you how to create a template-driven form. The control elements in the form are bound to data properties that have input validation. The input validation helps maintain data integrity and styling to improve the user experience.
	
	Template vs Reactive forms
		Angular supports two design approaches for interactive forms. Template-driven forms allow you to use form-specific directives in your Angular template. Reactive forms provide a model-driven approach to building forms.

		Template-driven forms are a great choice for small or simple forms, while reactive forms are more scalable and suitable for complex forms. For a comparison of the two approaches, see Choosing an approach
						
		Ex.
			Templateform.ts :
				import { JsonPipe } from '@angular/common';
				import { Component } from '@angular/core';
				import { FormsModule, ReactiveFormsModule } from '@angular/forms';
				import { RouterLink } from '@angular/router';


				// this is the schema of the template form
				// make for the reset front side or back side value in the ts or html file
				export class Actor {
				  constructor(public email: string, public psw: string, public skills: string) {}
				}

				@Component({
				  selector: 'app-templateform',
				  imports: [RouterLink, FormsModule, JsonPipe],
				  templateUrl: './templateform.html',
				  styleUrl: './templateform.css',
				})
				export class Templateform {
				  fname = ''; // for this single input

				  // Sample Template Form
				  // for this multiple inputs
				  user = {
					email: '',
					psw: '',
					skills: '',
				  };

				  skill = ['Dance', 'Swim', 'Lead', 'Communicate', 'Other'];
					
					// this a fired when the form is submitted
				  submit(form: any) {
					console.log(form.value);
				  }

				  // this is the reset form from the front and to backend side
				  // when this fired that time html or ts side data is become null
				  model = new Actor('','','');
				  ResetForm(){
					this.model = new Actor('','','');
				  }
					// html side responce which like form is submitted 
				  // responce form submission
				  sumitted = false;
				  reposnce(){
					console.log('submitted !')
					this.sumitted = true;
				  }
					// for this toggle value of the submitted true or false
				  submittoggle(){
					this.sumitted = !this.sumitted;
				  }
				}
			
			Templateform.html :
				<h1>Template Form :</h1>
				<h2>Simple Template Form :</h2>
				First Name : <input type="text" [(ngModel)]="fname" ] /> // put the fname in ths ngModel for the hadle input
				<p>{{ fname }}</p>
				<hr />

				<h3>Sample Template Form :</h3> 
				<h4>Apply Class On Required & Error Message Display</h4>
				<!-- NgForm → creates a FormGroup
					NgModel → creates FormControls -->
				<div>

				<form #sujalform="ngForm" (ngSubmit)="submit(sujalform)">
				  <div class="d-flex flex-column">
					<div>
					  <label for="email">Email : </label>
					  <input
						type="text"
						[(ngModel)]="user.email"
						id="email"
						class="form-control"
						name="email"
						required
						#email="ngModel" // for the show error 
					  />
					  // here div is hidden when the #email is valid 
					  <div [hidden]="email.valid || email.pristine" class="alert">
						<p>Email Required !</p>
					  </div>
					</div>

					<div>
					  <label for="psq">Password : </label>
					  <input
						type="text"
						[(ngModel)]="user.psw"
						name="psw"
						class="form-control"
						required
						#psq="ngModel" // same here password
					  />
					  <span [hidden]="psq.valid || psq.pristine">
						<p class="alert alert-danger">Password Is Requried !</p>
					  </span>
					</div>

					<div class="mt-3">
					  <label for="">Skill : </label>
					  <select id="" class="form-select" [(ngModel)]="user.skills" name="skills" required>
						<option value="">Select Skill</option>
						@for (item of skill; track $index) {
						<option value="{{ item }}">{{ item }}</option>
						}
					  </select>
					</div>
					<div class="m-4">
					  <button type="submit" class="btn btn-success">Save</button>
					  <button (click)="ResetForm(); sujalform.reset()" class="btn btn-primary ml-2">Reset</button>
					</div>
				  </div>
				</form>
				</div>

				<p>{{ user | json }}</p>

				<hr />
				<h2>Form Submitted Responce : </h2>
				
				// here apply the form submitted condition if form is submited then fedback is seen.
				<div [hidden]="sumitted">
				  <form (ngSubmit)="reposnce()">
					<div class="d-flex flex-column">
					  <label for="test">Form Submitted ? Click Button</label>
					  <button class="btn btn-success w-25">Check !</button>
					</div>
				  </form>
				</div>

				  @if (sumitted) {
					<p class="alert alert-success">Form Submitted SuccessFully !</p>
				  }

				@if (sumitted) {
				  <button class="btn btn-dnager" (click)="submittoggle()" >Toggle Form Submission</button>
				}

				<hr>
				<div style="margin-top: 10px" class="btn btn-danger w-100">
				  <a [routerLink]="['']" class="text-white">Back</a>
				</div>
				
○ Validating form input :
	- You can improve overall data quality by validating user input for accuracy and completeness. This page shows how to validate user input from the UI and display useful validation messages, in both reactive and template-driven forms.
	
	○ Validating input in template-driven forms :
			Validator functions can be either synchronous or asynchronous.

			Validator type	
				○ Sync validators	: Synchronous functions that take a control instance and immediately return either a set of validation errors or null. Pass these in as the second argument when you instantiate a FormControl.

				○ Async validators	: Asynchronous functions that take a control instance and return a Promise or Observable that later emits a set of validation errors or null. Pass these in as the third argument when you instantiate a FormControl.
				
	○ Built-in validator functions :
		- by using the validator.required minlength email etc it's a provided by the angular so we can use direct into the angular app.
		- main thing is use validator property.
		
	○ Defining custom validators :
		- The built-in validators don't always match the exact use case of your application, so you sometimes need to create a custom validator.
		- here we can make the own custom validator.
		- when we want to apply own ideal validation in the form the we use the custom validation concept by usoing ValidatorFn.
		
		Ex.
			first we make the one function which is apply on the form and all custom login is written in this function.
			
			unambiguousRoleValidator function :
				export function forbiddenAdminValidator(testdata: string[]): ValidatorFn { //ValidatorFn is use for custom logic buiding
				  return (control: AbstractControl): ValidationErrors | null => { // AbstractControl is the base class for all Angular form controls.
					if (!control.value) {
					  return null;
					}
					const value = control.value;
					const isForbidden = testdata.map((user) => user.toLowerCase()).includes(value);
					return isForbidden ? { forbiddenAdmin: true } : null;
				  };
				}
				
			in the ts file :
				  blockedUsers = ['admin', 'superadmin', 'root'];

					 userform = new FormGroup({
						fname: new FormControl(this.user.fname, [
						  Validators.required,
						  Validators.minLength(3),
						  forbiddenAdminValidator(this.blockedUsers), // custom function is apply here
						])
					  });
					  
					 get fname(){ // this is return the current value of the fname
						return this.userform.get('fname')
					  }
					  
			html file :
								
				<form [formGroup]="userform">
				  <label for="fname">Check Error :</label>
				  <input type="text" id="fname" class="form-control" formControlName="fname" />

				  @if (fname?.invalid && (fname?.dirty || fname?.touched)) {
				  <div class="text-danger">
				  // here error declare if required related validation fire then show this message 
					@if (fname?.hasError('required')) {
					<div>Name is required</div>
					} @if (fname?.hasError('minlength')) {
					<div>Minimum length is 3</div>
					} @if (fname?.hasError('forbiddenAdmin')) { // if forbiddenAdmin validation is fired then this div is shown
					<div>This name is not allowed</div>
					}
				  </div>
				  }
				</form>
				<hr />
			
	○ Adding custom validators to template-driven forms :
		- in this concept all things are same login are same and way of apply is same but some changes are apply here and we make the custom validation and apply on the template form.
		
		Ex.
			first of the we make the Directive and export the class
				
				@Directive({
				  selector:'[appnotallowword]', // Direct Name
				  providers:[{
					provide:NG_VALIDATORS, // provide
					useExisting:unwanteddetect, // use this class
					multi:true
				  }]
				})

				
				// make the class and impletemeted Validator
				export class unwanteddetect implements Validator{

				  // custom validation
				  @Input('appnotallowword') blocklist:string[]=[];
					
				  validate(control: AbstractControl): ValidationErrors | null {
					if(!control.value)return null;
					const val =control.value.toLowerCase()
					const isworld = this.blocklist.map(user=>user.toLowerCase()).includes(val);
					return isworld ? {isdeitect:true}:null;
				  }
				} // logic are same but some chages we make here.
				
			import this Directive in the componanent file :
				  imports: [FormsModule,unwanteddetect],
			
					export class Validationinputs {
					  user = { fname: '' };
					  blockuser = ['sujal','block','admin'];
					}
					
			html file :
				<h2>Custom Validation Template Form :</h2>

				<form #customform="ngForm">
				  <label for="fname">Custom Error :</label>
				  <input
				  type="text"
				  id="fname"
				  name="fname"
				  class="form-control"
				  required
				  [(ngModel)]="user.fname"
				  
				  // apply here custom validation and give the value blockuser
				  [appnotallowword]="blockuser"
				  #worldcollec = "ngModel"> // reference of the input element

					// check here input touched invalid dirty like
				  @if (worldcollec.invalid && (worldcollec.touched || worldcollec.dirty)) {
					<div>
					  @if (worldcollec.hasError('required')) {
						<div>Name Is Required !</div>
					  }
					  // if the custom error detect here so show the message
					  @if(worldcollec.hasError('isdeitect')){
						<div>Name Is Not Allowed !</div>
					  }
					</div>
				  }
				</form>


	
		○ Control status CSS classes :
			✅ 1. Validation State
				1).ng-valid
				✔ Control passes all validations

				.ng-valid {
				  border: 2px solid green;
				}

				2).ng-invalid
				❌ Control fails any validation
				
				.ng-invalid {
				  border: 2px solid red;
				}

				3).ng-pending
				⏳ Validation is running (mostly async validators)

				.ng-pending {
				  border: 2px solid orange;
				}

			✅ 2. Value Change State
				1).ng-pristine

				🆕 User has NOT changed the value yet
				.ng-pristine {
				  background-color: #f9f9f9;
				}

				2).ng-dirty
				✍ User HAS changed the value

				.ng-dirty {
				  background-color: #fff;
				}

			✅ 3. Interaction State
				1).ng-untouched
				🖱 Control never focused

				2).ng-touched
				👆 Control focused and blurred

				.ng-touched.ng-invalid {
				  border-color: red;
				}


			✔ Most common real-world usage

			✅ 4. Form-Level Class
				1).ng-submitted

				📤 Applied ONLY on <form>
				✔ When form is submitted

				form.ng-submitted input.ng-invalid {
				  border: 2px solid red;
				}

			🧠 Real-World Best Practice Styling
				🔥 Recommended Pattern
				input.ng-touched.ng-invalid {
				  border: 2px solid #dc3545;
				}

				input.ng-touched.ng-valid {
				  border: 2px solid #28a745;
				}
				
		○ Cross-field validation :
			- A cross-field validator is a custom validator that compares the values of different fields in a form and accepts or rejects them in combination. For example, you might have a form that offers mutually incompatible options, so that if the user can choose A or B, but not both. Some field values might also depend on others; a user might be allowed to choose B only if A is also chosen.
			- in this concept we validate more than 1 input field, like username and passwrod is not same, or fname & lname is not same etcc.
			- here we make the fname and lname is not a same logic apply here.
			
			
				1) Adding cross-validation to reactive forms :
					- cross validation is apply one reactive form :
						Ex.
							first we make the const for the cross validation.
								export const testing:ValidatorFn=(constrol:AbstractControl,):ValidationErrors|null=>{
								  const fname = constrol.get('fname');
								  const lname = constrol.get('lname');
								  return fname && lname && fname.value === lname.value ? {ismatch:true} : null;
								}
							
							ts file :
								export class Validationinputreactive {
								  crossfieldvalidation = new FormGroup(
									{
									  fname: new FormControl(""),
									  lname: new FormControl(""),
									},
									{ validators: testing }
								  );
								}
							
							html File :
								<form [formGroup]="crossfieldvalidation">
								  <h4>Check Cross Validation Field(Fname & Lname Not Same) :</h4>
								  <div>
									<label for="fname">Fname : </label>
									<input id="fnmae" name="lname" class="form-control" formControlName="fname" />
								  </div>
								  <div>
									<label for="lname">Lname : </label>
									<input id="lname" name="lname" class="form-control" formControlName="lname" />
								  </div>
								</form>
								// apply all full form
								@if (crossfieldvalidation.hasError('ismatch') && (crossfieldvalidation.dirty && crossfieldvalidation.touched)) {
								  <div >
									Name cannot match role or audiences will be confused.
								  </div>
								}
								
				2) Adding cross-validation to template-driven forms :
					Ex.
						@Directive({
						  selector:'[appnamesame]', // directive name
						  standalone:true,
						  providers:[{
							provide:NG_VALIDATORS,
							useExisting:crossvalidation,
							multi:true
						  }]
						})


						export class crossvalidation implements Validator{
						  validate(control: AbstractControl): ValidationErrors | null {
							const fname = control.get('fname'); // get the fname input field
							const lname = control.get('lname'); // get the lname input field

							if(!fname && !lname) return null;
							
							// if the fname & lname are same then return true ad throw erro message
							return fname?.value === lname?.value ? {iscross:true} : null; 
						  }
						}
					
					ts file :
						  imports: [FormsModule,crossvalidation],
											 
							export class Validationinputs {
							  // cross validation
							  crossvalid = {fname:'',lname:''}
							}
						
					html file :
						<h2>Cross Validation in Template Form :</h2>
						// Directive name write here
						<form #crossform="ngForm" appnamesame>
						  <div>
							<label for="fname" class="from-label">First Name : </label>
							<input
							type="text"
							id="fname"
							name="fname"
							required
							[(ngModel)]="crossvalid.fname"
							class="form-control"
							#fname="ngModel" // reference name
							>
						  </div>
						  <div>
							<label for="lname" class="from-label">Last Name : </label>
							<input
							type="text"
							id="lname"
							name="lname"
							required
							[(ngModel)]="crossvalid.lname"
							class="form-control"
							#lname="ngModel" // referece name
							>
						  </div>
						</form>
						// apply cross validation full of form
						// here if the iscross named error is true then show message
						@if (crossform.hasError('iscross') && (fname.touched || lname.touched)) {
						  <div class="text-danger">
							First & Last Name Is Same !
						  </div>
						}
						
			
	○ Creating asynchronous validators :
		🧠 Difference: Sync vs Async Validator
			Sync Validator					Async Validator
			Runs immediately				Runs asynchronously
			Returns ValidationErrors | null	Returns Promise or Observable
			No server calls					Often API calls
			Instant							Takes time
			Status = VALID / INVALID		Status = PENDING first
		
		
		○ Implementing a custom async validator :
			○ Reactive Form :(by using AsyncValidator)
				ts file :
				
					- @Injectable({providedIn:'root'})
						export class blockusers implements AsyncValidator{
						  validate(control: AbstractControl): Promise<ValidationErrors | null> | Observable<ValidationErrors | null> {
							return new Promise(res=>{
							  setTimeout(() => {
								if(control.value === "sujal"){res({adminBlocked:true})}
								else {res(null)}
							  }, 500)
							})
						  }
						}
						
						export class Validationinputreactive {
						  block = inject(blockusers);
						  username = new FormControl('',[],[this.block.validate.bind(this.block)]   // async validator(basic)
						}

				html file :
					<h2>Creating asynchronous validators : </h2>
					 <input [formControl]="username" placeholder="Username" />

					@if (username.pending) {
					  <div>Checking...</div>
					}

					@if (username.hasError('adminBlocked')) {
					  <div>Username "admin" is not allowed</div>
					}
			
			○ Template Form :
				Directive File :	
					@Directive({
					  selector:'[asynvalid]',
					  standalone:true,
					  providers:[
						{
						  provide:NG_ASYNC_VALIDATORS, // use must be like this
						  useExisting:Blockuserdetect,
						  multi:true
						}]
					})

					export class Blockuserdetect implements AsyncValidator{ // implements like this
					  validate(control: AbstractControl): Promise<ValidationErrors | null> | Observable<ValidationErrors | null> {
						return of(control.value).pipe(
						  delay(1000),
						  map(val=>val=== 'admin'? {adminBlocked:true}:null)
						);
					  }
					}
						
			○ Componet File :
			
				imports: [FormsModule,RouterLink,Blockuserdetect]// import first
				export class Validationinputs {
				  // async validation
				  asyncvalid = {username:''}
				}
			
			○ html file :
				<hr>
				<div>
				  <h2>Async Validatio Form : </h2>
				  <form #asyncf="ngForm">
					<input
					type="text"
					[(ngModel)]="asyncvalid.username"
					asynvalid // calling directive
					required
					id="username"
					name="username"
					#username="ngModel" // reference
					>

				  </form>
				   @if (username.pending) { // if the pending then message show 
					<div>Checking...</div>
				  }
				  @if (username.hasError('adminBlocked')) { // if the error occur then messageshow
					<div>Username "admin" is not allowed</div>
				  }
				</div>
		
		○ [ngModelOptions]="{updateOn:'change'}" :
			- it's most usefull feature of the asyncvalidation apply on the input element.
			○ The Problem :
				1.By default, Angular runs all validators after every value change.
				2.For sync validators (like required or pattern) → no problem, fast.
				3.For async validators (like checking username availability via HTTP) → huge performance cost:
				4.Sends a request on every keystroke
				5.Backend could be overloaded
				6.User sees flickering “checking…” messages
				
			🔹 The Solution: updateOn
				Angular allows us to control when a validator runs using updateOn.

				3 Options
				Value		Meaning
				'change'	Default → run on every keystroke
				'blur'		Run only when user leaves the input (recommended for async validators)
				'submit'	Run only when form is submitted
				
				Ex.
					<form #asyncf="ngForm">
						<label for="username">Username : </label>
						<input
						type="text"
						[(ngModel)]="asyncvalid.username"
						asynitem
						required
						id="username"
						name="username"
						#username="ngModel"
						class="form-control"
						[ngModelOptions]="{updateOn:'change & blur & submit'}"
						>
					  </form>
					   @if (username.pending) {
						<div>Checking...</div>
					  }
					  @if (username.hasError('adminBlocked')) {
						<div>Username "admin" is not allowed</div>
					  }
					  
○ Building dynamic forms :
	- dynamic form is the createdby the one declared model structure and apply only little bit code in the html.
	- main work are handle by the model structure.
	- in simple making html form when we want to change the some specific elements then change whole html code but here make one model and change model structure and automatically change the html code.
	- so fastest way to declare the html form in angular.
	 Ex.
		(first of all make basic form )
		dynamicformmodel.ts :
			// form structure and apply on html page
			export interface sujalquestion {
			  key: string;
			  label: string;
			  type: 'text' | 'email' | 'number';
			  required: boolean;
			}
		
		Dynamicforms.ts :
			import { Component, OnInit } from '@angular/core';
			import { FormControl, FormGroup, ReactiveFormsModule, Validators } from '@angular/forms';
			import { question, selfmade, sujalquestion } from './dynamicmodel';
			import { CommonModule, JsonPipe } from '@angular/common';

			@Component({
			  selector: 'app-dynamicforms',
			  imports: [ReactiveFormsModule, CommonModule],
			  templateUrl: './dynamicforms.html',
			  styleUrl: './dynamicforms.css',
			})
			
			export class Dynamicforms implements OnInit {
			  sujalform!: FormGroup; // full form
			  // all declared html attribute data
			  sujalqu: sujalquestion[] = [
				{ key: 'name', label: 'Full Name', type: 'text', required: true },
				{ key: 'email', label: 'Email Address', type: 'email', required: true },
				{ key: 'experience', label: 'Experience (Years)', type: 'number', required: false },
			  ];
			  
			  ngOnInit(): void {
				const group2: any = {}; // temp variable fro consume form data

				this.selfmadeData.forEach((res) => {
				// insert the data in the group
				  group2[res.key] = new FormControl('', res.required ? Validators.required : []);
				});
				this.selfmadeForm = new FormGroup(group2);
			  }
			  
			  submit2() {
				console.log(this.selfmadeForm.value);
			  }
			}
			
		html form :
			<h3>Self Made Dynamic Form :</h3>

			<div>
			  <form [formGroup]="selfmadeForm" (ngSubmit)="submit2()" class="border border-secondary p-3 w-50 mx-auto d-block">
			  // looping in the sujalqu
				@for (i of selfmadeData; track i.key) { @if (i.type == 'text') {
				<label for="{{ i.key }}">{{ i.label }}</label>
				<input type="text" id="{{ i.key }}" class="form-control" [formControlName]="i.key" />
				} @if (i.type == 'number') {
				<label for="{{ i.key }}">{{ i.label }}</label>
				<input type="number" id="{{ i.key }}" class="form-control" [formControlName]="i.key" />
				} @if (i.type == 'select') {
				<div>
				  <label for="">{{i.label}}</label>
				  <select class="form-control" [formControlName]="i.key">
					@for (s of i.selectdata; track s.skey) {
					<option value="{{ s.svalue }}">{{ s.svalue }}</option>
					}
				  </select>
				</div>
				} }

				<button type="submit" class="btn btn-success mx-auto d-block mt-2">Save</button>
			  </form>
			</div>
			
			(advanced example) written in the git hub links

○ Understanding communicating with backend services using HTTP : 
	🌐 Communicating with backend services using HTTP (Angular)

		Most front-end applications need to talk to a server to:
			fetch data (GET)
			send data (POST)
			update data (PUT/PATCH)
			delete data (DELETE)
			Angular does this using the HttpClient service from
			@angular/common/http.
				
	○ Setting up HttpClient :
		| Option                        | Simple meaning                |
		| ----------------------------- | ----------------------------- |
		| `withFetch()`                 | Modern fetch API use karo     |
		| `withInterceptors()`          | Custom interceptors add karo  |
		| `withInterceptorsFromDi()`    | DI wale interceptors use karo |
		| `withRequestsMadeViaParent()` | Parent HttpClient use karo    |
		| `withJsonpSupport()`          | Old JSONP support             |
		| `withXsrfConfiguration()`     | Security enable               |
		| `withNoXsrfProtection()`      | Security disable              |
	
	○ Httpclient module based configuration : (Ols(NgModule-based HttpClient) replaced new(provideHttpClient()) )
		| Old NgModule                         | New provideHttpClient                         |
		| ------------------------------------ | --------------------------------------------- |
		| `HttpClientModule`                   | `provideHttpClient(withInterceptorsFromDi())` |
		| `HttpClientJsonpModule`              | `withJsonpSupport()`                          |
		| `HttpClientXsrfModule.withOptions()` | `withXsrfConfiguration()`                     |
		| `HttpClientXsrfModule.disable()`     | `withNoXsrfProtection()`                      |

	
	○ Making HTTP requests :
		- by inject HttpClient or provideHttpClient() are used to fetching data or making http request.
		- in the service file we can only hit get,post,put,delete etc request, but componanent file we can subscribe this requesrt without subscribe not hiting the request in the background.
		
		Fetching JSON data :
			
			Ex.
				usersevice.ts : (service file):
					import { HttpClient } from '@angular/common/http';
					import { inject, Injectable } from '@angular/core';
					import { Observable } from 'rxjs';

					@Injectable({ providedIn: 'root' })
					export class userservice {
					  constructor(private http: HttpClient) {}

					  private apiUrl = 'https://jsonplaceholder.typicode.com/users';

					  getUsers(): Observable<any> {
						return this.http.get(this.apiUrl);
					  }
					}
				
				componanent file :
					export class Httpreq {
					  constructor(private usersService: userservice) { // inject via constructor userservice
						// DI happens here 👈
					  }
					   loadUsers() {
						this.usersService.getUsers().subscribe(data => { // subscribe must be here
						  console.log('Users from API:', data); // console  the data
						});
					  }
					}
					
				html file :
					<button (click)="loadUsers()">Load Users</button>
		
	○ Fetching other types of data :
		- 	responseType value	Returned response type
			'json' (default)	JSON data of the given generic type (by default responce)
			'text'				string data (as a text)
			'arraybuffer'		ArrayBuffer containing the raw response bytes(as a binary for sharing video image data etc..)
			'blob'				Blob instance
		Ex.
			@Injectable({ providedIn: 'root' })
				export class userservice {
				  constructor(private http: HttpClient) {}

				  private apiUrl = 'https://jsonplaceholder.typicode.com/users';

				  getUsers(): Observable<any> {
					return this.http.get(this.apiUrl,{responseType:'json(default) & blob & arraybuffer & text'});
				  }
				}
				
	○ Setting URL parameters :
		- in the api url if the paramaeter is available then get it and responce based on this paramaeter.
		- as get the data by id wise filter data for brand name wise etc all is happen by paramaeter.
		
		Ex.
			userservice.ts :
				import { HttpClient } from '@angular/common/http';
				import { inject, Injectable } from '@angular/core';
				import { Observable } from 'rxjs';

				@Injectable({ providedIn: 'root' })
				export class userservice {
				  constructor(private http: HttpClient) {}

				  private apiUrl = 'https://jsonplaceholder.typicode.com/users';

				  getUsers(sid : number): Observable<any> {
				  // give the paramaeter to the api url
					return this.http.get(this.apiUrl,{params:{id:sid}});
				  }
				}
				
			componanent.ts :
				import { Component, numberAttribute } from '@angular/core';
				import { userservice } from '../configservice';
				import { FormControl, FormGroup, FormsModule, ReactiveFormsModule } from '@angular/forms';

				@Component({
				  selector: 'app-httpreq',
				  imports: [ReactiveFormsModule],
				  templateUrl: './httpreq.html',
				  styleUrl: './httpreq.css',
				})

				export class Httpreq {
				  constructor(private usersService: userservice) {}
				  apiform= new FormGroup({
					ids :new FormControl('')
				  })
				   loadUsers() {
				   // get the id from the api url.
					const id = Number(this.apiform.get('ids')?.value);
					this.usersService.getUsers(id).subscribe(data => { // subscribe the api 
					  if(data.length ===0){
						console.log(404)
					  }else
					  console.log('Users from API:', data);
					});
				  }
				}
				
				html file :
					<p>httpreq works!</p> 
						<form [formGroup]="apiform" (ngSubmit)="loadUsers()"> // form for fetching the data by it's id
						  <input type="text" formControlName="ids">
						  <button type="submit">Load Users</button>
						</form>
											
	○ Custom parameter encoding :
		- Angular uses HttpUrlEncodingCodec to encode query params.
		- when we pass the url query then it's passed as a encode suppose when we pass test+200@gmail.com then it's passed the test%2B200%40gmail.com.
		- it's implement by the HttpParameterCodec.
		- it's decode as a :
				+   → %2B
				@   → %40
				&   → %26
				?   → %3F
				/   → %2F
				=   → %3D
				
		Ex.
			make customencoding named file :
				// custom-http-param-encoder.ts
				import { HttpParameterCodec } from '@angular/common/http';

				export class CustomHttpParamEncoder implements HttpParameterCodec {
				  encodeKey(key: string): string {
					// encode key using JS standard
					return encodeURIComponent(key); must be return encodeURIComponent
				  }
				  encodeValue(value: string): string {
					// encode value using JS standard
					return encodeURIComponent(value);
				  }
				  decodeKey(key: string): string {
					return decodeURIComponent(key);
				  }
				  decodeValue(value: string): string {
					return decodeURIComponent(value);
				  }
				}
			]
			
			○ Userservice.ts :
				
				import { HttpClient, HttpParams } from '@angular/common/http';
				import { Injectable } from '@angular/core';
				import { Observable } from 'rxjs';
				import { customencoding } from './customencodinghttps';

				@Injectable({ providedIn: 'root' })
				export class userservice {
				  constructor(private http: HttpClient) {}
				  private api = 'https://jsonplaceholder.typicode.com/users';

				  checkencode(email:any){
					const params = new HttpParams({encoder: new customencoding()})
					.set(`email`,email);
					return this.http.get('https://httpbin.org/get',{params});
				  }


				  getbyemailEncoded(email:any) {
					const params = new HttpParams({ encoder: new customencoding() })
					.set(`email`,email);
					return this.http.get('https://jsonplaceholder.typicode.com/users', { params }).subscribe(console.log);
				  }
				}
				
				Httpreq.ts :
					import { Component, numberAttribute } from '@angular/core';
					import { userservice } from '../configservice';
					import { FormControl, FormGroup, FormsModule, ReactiveFormsModule } from '@angular/forms';
					import { domainToASCII } from 'url';

					@Component({
					  selector: 'app-httpreq',
					  imports: [ReactiveFormsModule],
					  templateUrl: './httpreq.html',
					  styleUrl: './httpreq.css',
					})
					export class Httpreq {
					  constructor(private userservice: userservice) {}

					  // for testing and see encoded email or any string
					  encodeform= new FormGroup({
						encodeinput :new FormControl('')
					  })

							// PASSED : SUJAL GOPANI+2005 @GMAIL.COM
							// ANGULAR CONSIDER : SUJAL GOPANI%2B2005 %40GMAIL.COM
					  demoencode(){
						this.userservice.checkencode(this.encodeform.get('encodeinput')?.value).subscribe(data=>{
						  console.log(data);
						})
					  }

					  // fetch json by encode form email
					  getbyemail(){
						this.userservice.getbyemailEncoded(this.encodeform.get('encodeinput')?.value);
						// .subscribe(data=>{
						//   console.log(data)
						// })
					  }
					}
					
					Httpreq.html :
						<h4>Check The How The Original Value Angular Can Encode :</h4>
						<form [formGroup]="encodeform">
						  <label for="email">Enter Original Email : </label>
						  <input type="text" id="email" name="email" class="form-control" formControlName="encodeinput">
						  <button (click)="demoencode()">Click And See Encoded Email</button>
						</form>
						<hr>

						<h4>Fetch Json By Email Parameter Encode Form :</h4>
						<form [formGroup]="encodeform">
						  <label for="email">Enter Original Email : </label>
						  <input type="text" id="email" name="email" class="form-control" formControlName="encodeinput">

						  <button (click)="getbyemail()">Email Encoded</button>
						</form>
						
			○ Setting request headers :
				- if we want to send the meta data with the api responce then we use the header during the getting the api.
				- metadata is the immutable so it's not change if once time is created.
				- header is the send as a key : value pair.
				- header is visible in the browser in Network tab under header section.
				
				Ex.
					userservice.ts :
						getbyemailEncoded(email:any) {
							// declare the header with key : value pair
							const headers = {
							  'Made-By':'Sujal-Gopani'
							};
							const params = new HttpParams({ encoder: new customencoding() })
							.set(`email`,email);
							// here passed the header with params
							// that's it
							return this.http.get('https://jsonplaceholder.typicode.com/users',{ params,headers }).subscribe(console.log);
						  }
						  
			○ Interacting with the server response events :
				- For convenience, HttpClient by default returns an Observable of the data returned by the server (the response body). Occasionally it's desirable to examine the actual response, for example to retrieve specific response headers.
				- some of the case server response as a api result as json format but in this topic give the metadata like api status, headers, body etc...
				
				Ex.
					userservice.ts :
						getserverresponceevent(){
							const headers ={
							  'Sujal':'Angular'
							}
							// simply here declare the observe:'response'
							return this.http.get('https://jsonplaceholder.typicode.com/users?email=Sincere@april.biz',{headers,observe:'response'});
						  }
					
					httpreq.ts :						
					  getserverresponse(){
					  // so here we know the status code, body header etc..
						return this.userservice.getserverresponceevent().subscribe(data=>{
						  console.log(data.status);
						  console.log(data.body);
						  console.log(data.headers);
						})
					  }
					  // in the html when button click by this event that time server response with status body headers etc...
					  
			
			○ Receiving raw progress events :
				- Normally, when you use HttpClient, you only get one thing back:
					the final response (data from the server)
				- But Angular can also give you events during the request lifecycle, such as:
					when the request is sent
					upload progress
					download progress
					when headers arrive
					when the response is fully completed
				These are called raw HTTP events.
				
				type value						Event meaning
				HttpEventType.Sent				The request has been dispatched to the server
				HttpEventType.UploadProgress	An HttpUploadProgressEvent reporting progress on uploading the request body
				HttpEventType.ResponseHeader	The head of the response has been received, including status and headers
				HttpEventType.DownloadProgress	An HttpDownloadProgressEvent reporting progress on downloading the response body
				
				HttpEventType.Response			The entire response has been received, including the response body
				HttpEventType.User				A custom event from an Http interceptor.
			
				- that is the return real time uploading or downloading progress data.
				
				Ex.
					userservice.ts :
						import { HttpClient, HttpEvent } from '@angular/common/http';
						import { Injectable } from '@angular/core';
						import { Observable } from 'rxjs';

						@Injectable({ providedIn: 'root' })
						export class userservice {
						  constructor(private http: HttpClient) {}
							
							// here hit the post request to fake api for the upload document or any file
						  uploadFile(file :File):Observable<HttpEvent<any>>{
							// consume temporary file data
							const formdata = new FormData();
							formdata.append('file',file);
							const uploadapi  = 'https://api.escuelajs.co/api/v1/files/upload';
							
							// post request with api and file data
							return this.http.post(uploadapi,formdata,{
							  reportProgress:true, // reportProgress true it's help to handle real time uploading proccess
							  observe:'events' as const
							})
						  }

						}
				
					Httpreq.ts : (Component)
						import { Component } from '@angular/core';
						import { userservice } from '../configservice';
						import { ReactiveFormsModule } from '@angular/forms';
						import { HttpEventType } from '@angular/common/http';
						import { CommonModule } from '@angular/common';

						@Component({
						  selector: 'app-httpreq',
						  imports: [ReactiveFormsModule, CommonModule],
						  templateUrl: './httpreq.html',
						  styleUrl: './httpreq.css',
						})
						
						export class Httpreq {
						  constructor(private userservice: userservice) {}
						  
							// current upload count
						  progresscount = 0
						  // current uploading status
						  progressstatus = ''
							
							
						  addfile(event : Event){
							const input = event.target as HTMLInputElement;
							if(!input.files || input.files.length === 0){
							  return;
							}

							const file = input.files[0];
							this.uploadfile(file);
						  }
							
							// function for the handling real time uploading data 
						  uploadfile(file:File){
							this.userservice.uploadFile(file).subscribe((event)=>{
							  switch(event.type){ // when select or sent file to serve then show Upload Loading...
								case HttpEventType.Sent:
								  this.progressstatus = "Upload Loading....!";
								  break;

								case HttpEventType.UploadProgress : // show current number of uploading value
								  this.progresscount = Math.floor((event.loaded/(event.total?? 1))*100);
								  break;

								case HttpEventType.Response:// after uploading complete then Uploaded Data SuccessFully
								  this.progressstatus = "Uploaded Data SuccessFully Done !";
								  console.log(event.body);
								  break;
							  }
							})
						  }
						}
						
				httpreq.html :
					  <input type="file" (change)="addfile($event)"> // input as file and fire addfile function 
					  <div *ngIf="progresscount >0"> // show if the progresscount is > 0
						<progress max="100" [value]="progresscount"></progress> // progree bar 
						<span>{{progresscount}}%</span>
						<p>{{progressstatus}}</p>
					  </div>
					  
					  
			○ Handling request failure :
				- when the api responce consume the over timing, and consume the over timing then server throw the error, or erro message and show in the front end.
				- in this we use the timeout(minimum consume time), retry(retry fetch data), catchError(erro Function handle error), HttpErrorResponse.
				
				Ex.
					userservice.ts :
						import {
						  HttpClient,
						  HttpErrorResponse,
						} from '@angular/common/http';
						import { Injectable } from '@angular/core';
						import { catchError, Observable, retry, throwError, timeout } from 'rxjs';

						@Injectable({ providedIn: 'root' })
						export class userservice {
						  constructor(private http: HttpClient) {}
						  // error fail then handle
						  getdatawitherrorcheck(): Observable<any> {
							return this.http // fetch 500 comment data from internet
							  .get('https://jsonplaceholder.typicode.com/comments')
							  .pipe(timeout(147), retry(2), catchError(this.handleError)); // maximum time consume 147 mm seconds, retry time twise, catch error by below function
						  }
							
						  private handleError(erro: HttpErrorResponse) { // handle server side erro by HttpErrorResponse
							let msg = 'Something Went Wrong !';
							if (erro.status === 0) { // if the network is going off then msg show
							  msg = 'Network error or Server unreachanble';
							} else {
							  msg = 'Server Side Error !'; // other condition show server side error
							}

							return throwError(() => ({
							  msg, // error get by msg
							  status: erro.status, 
							}));
						  }
						}
					
					httpreq.ts :
						import { Component } from '@angular/core';
						import { userservice } from '../configservice';
						import { ReactiveFormsModule } from '@angular/forms';
						import { CommonModule } from '@angular/common';


						@Component({
						  selector: 'app-httpreq',
						  imports: [ReactiveFormsModule, CommonModule],
						  templateUrl: './httpreq.html',
						  styleUrl: './httpreq.css',
						})
						export class Httpreq {
						  constructor(private userservice: userservice) {}
						  // api response fail
						  ngOnInit(){
							this.geterrocheck()
						  }

						  loading = false;
						  errormsg = '';
						  edata : string[] = [];
						  geterrocheck(){
							this.loading = true;
							this.errormsg= '';
								
								// get the data function
							this.userservice.getdatawitherrorcheck().subscribe({
							  next:data=>{

								  this.edata = data;
								  this.loading = false;
								  console.log(this.edata)
							  },
							  error:err=>{
								this.loading= false;
								this.errormsg= err.msg;
							  }
							})
						  }

						  retry() { // if the api is fail then retry is used for re fetched data
							this.geterrocheck();
						  }
						}

				httpreq.html :
					<h2>Api Responce Fail Check :</h2>
				  <!-- <button (click)="geterrocheck()">Get Data</button> -->
				  <p *ngIf="loading">Loading users...</p>

				  <!-- Error -->
				  <div *ngIf="errormsg" class="alert alert-danger">
					{{ errormsg }}
					<button class="btn btn-sm btn-warning ms-2" (click)="retry()">
					  Retry
					</button>
				  </div>

				  <!-- Success -->
				  <ul *ngIf="!loading && !errormsg">
					<li *ngFor="let user of edata">
					  {{ user.length }}
					</li>
				  </ul>
				  <hr class="border border-secondary" />
	
		○ Advanced fetch options :
			🟢 1. Fetch Options (Overview)

				These are special settings you can pass in Angular when using the Fetch backend to control how HTTP requests behave.
				Example:

				this.http.get('/api/data', {
				  cache: 'no-cache',
				  priority: 'high',
				  mode: 'cors'
				}).subscribe(data => console.log(data));

			🟡 2. HTTP Caching Control (cache)

				Controls how the browser uses cached responses.

				Option	What it does	Use case
				force-cache	Always use cached response, even if stale	Config files, static JSON
				no-cache	Always check network first	Live data, dashboards
				only-if-cached	Use cache only, fail if missing	Offline apps, PWA
				reload	Ignore cache completely	Debugging, admin tools

				✅ Tip: Improves page speed and performance.

			🟡 3. Request Priority (priority)

				Controls which requests load first. Helps Core Web Vitals (LCP, INP).

				Option	What it does	Use case
				high	Load early	Hero image, main content
				low	Load later	Analytics, background requests
				auto	Browser decides	Normal requests

				✅ Tip: Use high priority for things users see first.

			🟡 4. Request Mode (mode)

				Controls cross-origin behavior.

				Option	What it does	Use case
				same-origin	Only allow requests to same domain	Internal sensitive APIs
				cors	Allow cross-origin with proper server headers	External APIs
				no-cors	Send request, but response is opaque	Images, scripts, fire-and-forget analytics
				🟡 5. Redirect Handling (redirect)

				Controls what happens when a request is redirected.

				Option	What it does
				follow	Automatically follow redirects (default)
				error	Throw an error if redirected
				manual	Handle redirect manually in code

				✅ Useful for login flows or security checks.

			🟡 6. Credentials Handling (credentials)

				Controls cookies / auth info sent with request.
				Option	What it does
				omit	Never send cookies
				same-origin	Send cookies for same origin only
				include	Always send cookies, even cross-origin

				✅ Important for login/authenticated APIs.

			🟡 7. Referrer (referrer)

				Controls what URL is sent as “Referrer”.

				Example: referrer: 'https://example.com/page1'
				Default: URL of the current page

			🟡 8. Referrer Policy (referrerPolicy)

				Controls how much referrer info is sent.
				Option	Meaning
				no-referrer	Don’t send any referrer
				origin	Send only origin (https://example.com)
				strict-origin-when-cross-origin	Full URL for same origin, only origin for cross-origin
				✅ Helps privacy & security.

			🟡 9. Integrity (integrity)

				Used to verify file integrity (hash check) for scripts, styles, etc.

				Example:
				this.http.get('/assets/app.js', { integrity: 'sha384-abc123...' })
				✅ Ensures file was not tampered with during download.

			🟢 💡 Beginner Cheat Sheet (All Together)
				Option			Simple Meaning				When to use
				--------------------------------------------------------------------
				cache			Use or ignore browser cache	Speed vs fresh data
				priority		Load early or late			Critical UI vs background
				mode			Cross-origin behavior		Internal vs external API
				redirect		What to do on redirect		Login / security flows
				credentials		Send cookies or not			Authenticated APIs
				referrer		URL sent as referrer		Analytics / tracking
				referrerPolicy	How much referrer info		Privacy
				integrity		Verify file hash			Security / CDN
				
			
			○ Http Observables :
				- Http Observables represent an asynchronous HTTP request that can be started, canceled, combined, and automatically cleaned up.
				
						| Concept                | Meaning                     |
						| ---------------------- | --------------------------- |
						| Cold Observable        | No request until subscribed |
						| Multiple subscriptions | Multiple HTTP calls         |
						| Unsubscribe            | Cancels request             |
						| async pipe             | Auto subscribe/unsubscribe  |
						| switchMap              | Cancels previous requests   |
						| Auto complete          | No memory leak (usually)    |

			○ Best practices :
				
				- While HttpClient can be injected and used directly from components, generally we recommend you create reusable, injectable services which isolate and encapsulate data access logic. For example, this UserService encapsulates the logic to request data for a user by their id:
				
					Ex.
						
						@Injectable({providedIn: 'root'})
						export class UserService {
						  private http = inject(HttpClient);
						  getUser(id: string): Observable<User> {
							return this.http.get<User>(`/api/user/${id}`);
						  }
						}
				
				- Within a component, you can combine @if with the async pipe to render the UI for the data only after it's finished loading:

					import { AsyncPipe } from '@angular/common';
					@Component({
					  imports: [AsyncPipe],
					  template: `
						@if (user$ | async; as user) {
						  <p>Name: {{ user.name }}</p>
						  <p>Biography: {{ user.biography }}</p>
						}
					  `,
					})
					export class UserProfileComponent {
					  userId = input.required<string>();
					  user$!: Observable<User>;
					  private userService = inject(UserService);
					  constructor(): void {
						effect(() => {
						  this.user$ = this.userService.getUser(this.userId());
						});
					  }
					}
					
	○ Reactive data fetching with httpResource :
		- httpResource enables reactive HTTP data fetching using Angular Signals, automatically handling re-fetching, cancellation, loading, and errors without manual subscriptions.
		
			| Feature         | Http Observable | httpResource |
			| --------------- | --------------- | ------------ |
			| Subscription    | Manual          | Automatic    |
			| Cancellation    | `unsubscribe()` | Automatic    |
			| Reactive inputs | RxJS            | Signals      |
			| Loading state   | Manual          | Built-in     |
			| Code size       | More            | Less         |
		
		- httpResource is a reactive wrapper around HttpClient that gives you the request status and response as signals. You can thus use these signals with computed, effect, linkedSignal, or any other reactive API. Because it's built on top of HttpClient, httpResource supports all the same features, such as interceptors.

	
		○ Using httpResource :
			- if we use the simple way for http Observable that is return only retur api data and in this also like make or declare the methods and subscribe the method where we need or use.
			- in simple language when use the observable then it's declare or subscribe then it's fetching complete and get the api responce data.
			- but the httpResource is declare once time after it's subscribe auto cancel auto or get the return data automatic.
			
			Ex.(simple basic example of the httpResource)
			componanent file :
				import { Component } from '@angular/core';
				import { userservice } from '../configservice';
				import { ReactiveFormsModule } from '@angular/forms';
				import { httpResource } from '@angular/common/http';
				import { CommonModule } from '@angular/common';

				@Component({
				  selector: 'app-httpreq',
				  imports: [ReactiveFormsModule, CommonModule],
				  templateUrl: './httpreq.html',
				  styleUrl: './httpreq.css',
				})
				export class Httpreq {
				  constructor(private userservice: userservice) {}
				  // use the httpResource
				  httpResource_Component = httpResource.text(
					() => 'https://jsonplaceholder.typicode.com/comments/1'
				  );

				  getdata() {
					this.httpResource_Component();
				  }
				}
				
			html file :
				<button (click)="getdata">Fetch Data</button>
				// here simplly declare the httpResource and it's subscribe auto when it's calling
				
			
			○ other style to declare the httpResource :
				user = httpResource(() => ({
				  url: `/api/user/${userId()}`,
				  method: 'GET',
				  headers: {
					'X-Special': 'true',
				  },
				  params: {
					'fast': 'yes',
				  },
				  reportProgress: true,
				  transferCache: true,
				  keepalive: true,
				  mode: 'cors',
				  redirect: 'error',
				  priority: 'high',
				  cache: 'force-cache',
				  credentials: 'include',
				  referrer: 'no-referrer',
				  integrity: 'sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GhEXAMPLEKEY=',
				  referrerPolicy: 'no-referrer',
				}));
				// here all work is done which is used in the Observable
				
				
			○ httpResource Control :
				hasvalue, error, isLoading etc..
				
				Ex.
					@if(user.hasValue()) { // if the api has value then it's reutrn
					  <user-details [user]="user.value()">
					} @else if (user.error()) { // if it's return error then print error
					  <div>Could not load user information</div>
					} @else if (user.isLoading()) { // during fetching data from the api server during this time print this
					  <div>Loading user info...</div>
					}
				
			○ Response types :
				httpResource.text(() => ({ … })); // returns a string in value() 
				httpResource.blob(() => ({ … })); // returns a Blob object in value()
				httpResource.arrayBuffer(() => ({ … })); // returns an ArrayBuffer in value()
				
			
			○ Response parsing and validation :
				- in the MVC style Model is the schema of the represent of the data result same thing are apply on this topic.
				- using zod (install zod by npm install zod) define the data represent rule if the data is the broken those rule then throw the error, it's same like a Model in the MVC architecture.
				
				Ex.
					service file :
						import {
						  HttpClient,
						  httpResource,
						} from '@angular/common/http';
						import { Injectable } from '@angular/core';
						import { z } from 'zod';
						
						// this is the define the rule of the api result data
						// here when first consider id,name,body and also id is always consider number as well as name,body
						export const Comment_Parsing_Validation = z.object({
						  id: z.number(),
						  name: z.string(),
						  body: z.string(),
						});


						@Injectable({ providedIn: 'root' })
						export class userservice {
						  constructor(private http: HttpClient) {}
						  commentApi = httpResource(() => `https://jsonplaceholder.typicode.com/comments/1`, {
							// here apply the that rule
							parse: Comment_Parsing_Validation.parse,
						  });

						  Parsing_Validating() {
								if (this.commentApi.hasValue()) {
								  console.log('Parsed Comment:', this.commentApi.value());
								}
							}
							console.log('200 OK');
						  }
					}
				
				componanentn file :
				  Get_Data_With_Validating_And_Parsing(){
					this.userservice.Parsing_Validating();
				  }
				  
				html file :
					 <button (click)="Get_Data_With_Validating_And_Parsing()" class="btn btn-success">Get By zod</button>
	
	○ Interceptors :
		- HTTP Interceptors are a very important concept in Angular, especially when working with APIs.
		- An HTTP Interceptor is a class that sits between your Angular app and the backend server.
		- It allows you to:
			Modify outgoing HTTP requests
			Handle incoming HTTP responses
			Catch errors globally
			Add logic like:
			Authentication tokens (JWT)
			Logging
			Loading spinners
			Error handling
		- Real-Life Analogy :
			Think of an interceptor like security at an airport ✈️:
				Every passenger (HTTP request) must pass through security
				Security can:
					Check passport (auth token)
					Scan luggage (request data)
					Stop suspicious passengers (errors)
					
		○ Basic Structure of an Interceptor :
			import { Injectable } from '@angular/core';
			import {
			  HttpEvent,
			  HttpHandler,
			  HttpInterceptor,
			  HttpRequest
			} from '@angular/common/http';
			import { Observable } from 'rxjs';

			@Injectable()
			export class AuthInterceptor implements HttpInterceptor {

			  intercept(
				req: HttpRequest<any>,
				next: HttpHandler
			  ): Observable<HttpEvent<any>> {

				// logic here
				return next.handle(req);
			  }
			}
			
			Ex.
				
				(Simple Basic Example)
				export function testingInte(
				  req: HttpRequest<unknown>,
				  next: HttpHandlerFn
				): Observable<HttpEvent<unknown>> {
				  console.log(req.url," Sujal");
				  return next(req);
				}
				
				(Modify Body)
				export function goodInterceptor(
				  req: HttpRequest<unknown>,
				  next: HttpHandlerFn
				):Observable<HttpEvent<unknown>>{
				  if(!req.body)
					return next(req);

				  const newbody ={
					...req.body,
					isAdmin :true
				  }

				  const newreq = req.clone({body:newbody})

				  return next(newreq);
				}
				
				
				Register In The Main.ts :
					
					bootstrapApplication(App,{
					  providers:[
						provideRouter(routes),
						provideHttpClient(withInterceptors([goodInterceptor])) // functional registration
					  ],
					})


○ Server-side & hybrid rendering :
	- Temporary Not Consider ...
	
○ Angular Aria :
	- Building accessible components seems straightforward, but implementing them according to the W3C Accessibility Guidelines requires significant effort and accessibility expertise.
	- Angular Aria is a collection of headless, accessible directives that implement common WAI-ARIA patterns. The directives handle keyboard interactions, ARIA attributes, focus management, and screen reader support. All you have to do is provide the HTML structure, CSS styling, and business logic!
	- when keybord, mouse is used then browser perofrm not properly by using this we will handle the keybord, mouse events.
	- Installation
		 ○ npm install @angular/aria
		 
		 Search and selection :
			Component		Description
			Autocomplete	Text input with filtered suggestions that appear as users type
			Listbox			Single or multi-select option lists with keyboard navigation
			Select			Single-selection dropdown pattern with keyboard navigation
			Multiselect		Multiple-selection dropdown pattern with compact display
			Combobox		Primitive directive that coordinates a text input with a popup
			
		Navigation and call to actions
			Component	Description
			Menu		Dropdown menus with nested submenus and keyboard shortcuts
			Menubar		Horizontal navigation bar for persistent application menus
			Toolbar		Grouped sets of controls with logical keyboard navigation
		
		Content organization
			Component	Description
			Accordion	Collapsible content panels that can expand individually or exclusively
			Tabs		Tabbed interfaces with automatic or manual activation modes
			Tree		Hierarchical lists with expand/collapse functionality
			Grid		Two-dimensional data display with cell-by-cell keyboard navigation
		
		When to use Angular Aria

			Angular Aria works well when you need accessible interactive components that are WCAG compliant with custom styling. Examples include:

			Building a design system - Your team maintains a component library with specific visual standards that need accessible implementations
			Enterprise component libraries - You're creating reusable components for multiple applications within an organization
			Custom brand requirements - The interface needs to match precise design specifications that pre-styled component libraries cannot easily accommodate
			
		When not to use Angular Aria
			Angular Aria might not fit every scenario:
			Pre-styled components - If you need components that look complete without custom styling, use Angular Material instead
			Simple forms - Native HTML form controls like <button> and <input type="radio"> provide built-in accessibility for straightforward use cases
			Rapid prototyping - When validating concepts quickly, pre-styled component libraries reduce initial development time
		
	○ Accordion :
		- An accordion organizes related content into expandable and collapsible sections, reducing page scrolling and helping users focus on relevant information. Each section has a trigger button and a content panel. Clicking a trigger toggles the visibility of its associated panel.
		- when we want to show output like sub menu labling or under main one topic sub topic we declare then it's usefull.
		
				
			
	 Ex.
		ts file :
			import { Component } from '@angular/core';
			// must import this class for the performing accordion in the browser
			import {
			  AccordionGroup,
			  AccordionTrigger,
			  AccordionPanel,
			  AccordionContent,
			} from '@angular/aria/accordion';
			import { FormsModule } from "@angular/forms";


			@Component({
			  selector: 'app-accordion',
			  imports: [AccordionGroup, AccordionTrigger, AccordionPanel, AccordionContent, FormsModule],
			  templateUrl: './accordion.html',
			  styleUrl: './accordion.css',
			})

			export class Accordion {}

			
		html file :
			<h2>Accordion :</h2>
				<!-- here if we want to apply only single panel trigger then option is stay false other wise for the multiple then true -->
				<div ngAccordionGroup [multiExpandable]="true"> <!-- ngAccordionGroup contain the all panel group -->
				  <h4>Single expansion mode :</h4>
				  <h3>
					<span ngAccordionTrigger panelId="q1" #trigger1="ngAccordionTrigger"> <-- ngAccordionTrigger is main base to triger sub base or || panelId is trigger opr open the content by it's id -->
					  Something Here
					  <span
						aria-hidden="true"
						class="expand-icon" 
						[class.expand-icon__expanded]="trigger1.expanded()" <!-- when the trigger1 is open then apply the expand-icon__expanded class by custom css class -->
						translate="no"
						>▲</span>
					</span>
				  </h3>
				  <div ngAccordionPanel panelId="q1"> <!-- ngAccordionPanel or panelId is sub content which is triggered by the panelId -->
					<ng-template ngAccordionContent>
					  <p>Lorem ipsum dolor sit amet.</p>
					</ng-template>
				  </div>
					
					<!-- same panel like above first panel -->
				  <h3>
					<span ngAccordionTrigger panelId="q3" #trigger2="ngAccordionTrigger"> Just Seen
					  <span
						aria-hidden="true"
						class="expand-icon"
						[class.expand-icon__expanded]="trigger2.expanded()" 
						translate="no"
						>▲</span>
					</span>
				  </h3>

				  <div ngAccordionPanel panelId="q3">
					<ng-template ngAccordionContent>
					  <p>Lorem ipsum dolor sit amet.</p>
					</ng-template>
				  </div>
				  <div>

				  </div>
				</div>
			
		css file :
			[ngAccordionGroup]{ /* apply on full ngAccordionGroup */
			  width: 500px;
			  padding: 10px;
			}
			
			h3:focus-within, /* key bord foucs */
			h3:hover /*  mouse hover*/ {
			  background-color: #e6d4d7;
			}

			h3:first-of-type /* first of h3 select only first h3 of the line */ {
			  border-radius: 1rem 1rem 0 0;
			  border-block-start: 1px solid black;
			}

			h3:last-of-type { /* last of the all h3 */
			  border-block-end: 1px solid black;
			  border-radius: 0 0 1rem 1rem;
			}

			h3:last-of-type:has([aria-expanded='true']) { /* if the last h3 is trigered or expanded then apply this css */
			  border-block-end: 0;
			  border-bottom-left-radius: 0;
			  border-bottom-right-radius: 0;
			}

			p { /* apply on all p(paragraph) */
			  margin: 0;
			  padding: 0 1.5rem 1.5rem 1.5rem;
			  color: #756164;
			  font-size: 0.875rem;
			  border-inline: 1px solid var(--quinary-contrast);
			  border-radius: 0 0 1rem 1rem;
			  border-block-end: 1px solid var(--quinary-contrast);
			  margin-block-end: 1rem;
			}

			[ngAccordionTrigger] { /* apply on ngAccordionTrigger */
			  display: flex;
			  align-items: center;
			  justify-content: space-between;
			  outline: none;
			  cursor: pointer;
			  padding: 1.5rem;
			}

			[ngAccordionTrigger][aria-disabled='true'] {
			  opacity: 0.5;
			  cursor: default;
			}


			.expand-icon { /* appy on where we write this class in the html */
			  font-size: 1.3rem;
			  color: rgb(208, 128, 141);
			  transition: transform .3s ease-out;
			}

			.expand-icon__expanded { /* same apply like above expand-icon class type*/
			  transform: rotate(180deg);
			}
	
	○ Autocomplete :
		- An accessible input field that filters and suggests options as users type, helping them find and select values from a list.
		- 
		
	○ Combobox :
				
			
○ Animating your applications with animate.enter and animate.leave :
	- Well-designed animations can make your application more fun and straightforward to use, but they aren't just cosmetic. Animations can improve your application and user experience in a number of ways:

		Without animations, web page transitions can seem abrupt and jarring
		Motion greatly enhances the user experience, so animations give users a chance to detect the application's response to their actions
		Good animations can smoothly direct the user's attention throughout a workflow
		Angular provides animate.enter and animate.leave to animate your application's elements. These two features apply enter and leave CSS classes at the appropriate times or call functions to apply animations from third party libraries. animate.enter and animate.leave are not directives. They are special API supported directly by the Angular compiler. They can be used on elements directly and can also be used as a host binding.

		On this page
		animate.enter
		animate.leave
		Element removal order matters
		Event Bindings, Functions, and Third-party Libraries
		Compatibility with Legacy Angular Animations
		Testing
		More on Angular animations
	- Enter & Leave : when the animation add in the DOM Then Enter & When the Remove From DOM then Leave.
	
		Ex.
			animate.enter & Leave :
				ts File :
					import { Component, signal } from '@angular/core';
					@Component({
					  selector: 'app-animatingapp',
					  imports: [],
					  templateUrl: './animatingapp.html',
					  styleUrl: './animatingapp.css',
					})
					export class Animatingapp {
					  isShown = signal(false);

					  Toggle(){
						this.isShown.update((i)=>!i);
						console.log(this.isShown);
					  }
					}

					
				Html File : 
					<h3>Using animate.enter & animate.leave :</h3>
					<button type="button" class="toggle-btn btn btn-secondary m-3" (click)="Toggle()">Click</button>

					@if (isShown()) {
					  <div
						class="enter-container w-50 mx-auto d-block"
						animate.enter="enter-animation"
						animate.leave="leave-animation"
					  >
						<p>The box is entering & leaving</p>
					  </div>
					}

				Css File :
					:host {
					  display: block;
					  height: 200px;
					}

					/* Card UI */
					.enter-container {
					  border: 2px solid #dddddd;
					  margin-top: 1em;
					  padding: 20px;
					  font-weight: bold;
					  font-size: 20px;
					}

					.enter-container p {
					  text-align: center;
					}

					/* ENTER : Bottom ➜ Top */
					.enter-animation {
					  animation: enterFromBottom 0.6s ease-out;
					}

					@keyframes enterFromBottom {
					  from {
						opacity: 0;
						transform: translateY(30px);
					  }
					  to {
						opacity: 1;
						transform: translateY(0);
					  }
					}

					/* LEAVE : Top ➜ Bottom */
					.leave-animation {
					  animation: leaveToBottom 0.6s ease-in;
					}

					@keyframes leaveToBottom {
					  from {
						opacity: 1;
						transform: translateY(0);
					  }
					  to {
						opacity: 0;
						transform: translateY(30px);
					  }
					}

○ Animating your Application with CSS :
	- In this concept we learn the using only pure css perform the animation using transition.
	- here no need to js or animation properties.
	
	Ex.
		ts file :
			 isOpen = signal(false);
			  ToggleOpen(){
				this.isOpen.update((i)=>!i);
			  }

		
		html file :
			<h3>Animating your Application with CSS</h3>

			<button type="button" class="toggle-open-close-btn" (click)="ToggleOpen()">Toggle Open/Close</button>

			<div class="open-close-container" [class.open]="isOpen()">
			  <p>The Box Is  Now {{isOpen() ? 'Open' : 'Closed'}} !</p>
			</div>

		css file :
			.container {
			  display: grid;
			  grid-template-rows: 0fr;
			  overflow: hidden;
			  transition: grid-template-rows 1s;

			}

			.container.openauto {
			  grid-template-rows: 1fr;
			}

			.container .content {
			  min-height: 0;
			  transition: visibility 1s;
			  padding: 0 20px;
			  visibility: hidden;
			  margin-top: 1em;
			  font-weight: bold;
			  font-size: 20px;
			  background: #3b82f6;
			  color: #ebebeb;
			  overflow: hidden;
			}
			.container.openauto .content {
			  visibility: visible;
			}
			.toggle-btn {
			  background: transparent;
			  border: 1px solid var(--primary-contrast, black);
			  color: var(--primary-contrast, black);
			  padding: 10px 24px;
			  border-radius: 8px;
			  cursor: pointer;
				margin: 10px;

			}
			
			
○ Drag & Drop :
	○ Basic Drag (cdkDrag) :
		Use: Any element draggable banava
		<div cdkDrag>Drag me</div>
		Element freely drag થઈ શકે છે

	○ Drag with Drop List (cdkDropList)

		Use: List reorder કરવા

		<div cdkDropList (cdkDropListDropped)="drop($event)">
		  <div cdkDrag *ngFor="let item of list">{{item}}</div>
		</div>

		drop(event: CdkDragDrop<string[]>) {
		  moveItemInArray(this.list, event.previousIndex, event.currentIndex);
		}


	○ Same list માં items reorder થાય

		Transfer Between Two Lists

			Use: One list → another list move

			drop(event: CdkDragDrop<string[]>) {
			  if (event.previousContainer === event.container) {
				moveItemInArray(event.container.data, event.previousIndex, event.currentIndex);
			  } else {
				transferArrayItem(
				  event.previousContainer.data,
				  event.container.data,
				  event.previousIndex,
				  event.currentIndex
				);
			  }
			}
			➡️ Items move થાય (Cut + Paste)

	○ Copy Item Between Lists

		Use: Item copy કરવો (Original stay)

		copy(event: CdkDragDrop<string[]>) {
		  copyArrayItem(
			event.previousContainer.data,
			event.container.data,
			event.previousIndex,
			event.currentIndex
		  );
		}


		➡️ Original list unchanged, new list માં copy

	○ Selective Drop (Predicate)

		Use: Only specific items allow

		evenPredicate(item: CdkDrag<number>) {
		  return item.data % 2 === 0;
		}

		<div cdkDropList [cdkDropListEnterPredicate]="evenPredicate"></div>


		➡️ Only even numbers drop થશે

	○ Disable Drop Completely
		notAllowed() {
		  return false;
		}

		<div cdkDropList [cdkDropListEnterPredicate]="notAllowed"></div>
		No item allowed

	○ Restrict Drag Boundary

		Use: Drag only inside container

		<div class="boundary">
		  <div cdkDrag cdkDragBoundary=".boundary">Drag inside only</div>
		</div>

	○ Lock Drag Axis

		Use: Only X or Y movement

		<div cdkDrag cdkDragLockAxis="x">Only Horizontal</div>
		<div cdkDrag cdkDragLockAxis="y">Only Vertical</div>

	○ Drag Start Delay

		Use: Delay before drag start

		<div cdkDrag [cdkDragStartDelay]="1000">Drag after 1 sec</div>

		🔟 Disable Dragging
		<div cdkDrag [cdkDragDisabled]="true">Disabled</div>
		➡️ Drag possible નહીં

	🔑 Important Utilities
		Function	Use
		moveItemInArray	Reorder same list
		transferArrayItem	Move item
		copyArrayItem	Copy item
		CdkDragDrop	Drop event type
		CdkDrag	Drag reference

	📦 Required Imports
		import {
		  CdkDrag,
		  CdkDropList,
		  CdkDragDrop,
		  moveItemInArray,
		  transferArrayItem,
		  copyArrayItem
		} from '@angular/cdk/drag-drop';

	✅ Summary (One Line)

		Angular CDK Drag & Drop allows reorder, transfer, copy, restrict, filter & control drag behavior easily using directives and helper methods.


https://github.com/sujalgopani/AngularPractice.git for the crud op
				


				
				


						
						




		
		
								
				

			
		
			
			
			
			
			
			
			
			
			
			
			
			
			
