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
	
								

						
						Advanced component configuration.... 
