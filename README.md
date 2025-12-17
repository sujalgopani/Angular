selector -> componanent named define which is used in access componanent in other componanent
imports -> import the other side to componanent like header,footer [header,footer or any directives]
templateurl -> seprate the html file attach
styleurl -> seprate css file attach
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
		
		
		
		Other common Routing Tasks
		
		
	
