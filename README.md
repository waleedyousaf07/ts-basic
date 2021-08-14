# ts-basic
Basic typescript app

TS Basics:
-----------

- As the browsers only knows basic JS, after making a TS file,
use command `tsc my-ts-file.ts updated-js-file.js` for typescript conversion which
will make its JS version
- We can also avoid the target file if we wanna create the JS version with the same
name `tsc my-ts-file.ts`
- When we will make changes to out TS file, we'll have to update and make its JS version
with the same 'tsc' command. In order to avoid it, use `tsc my-ts-file.ts -w` which will
start to watch for the changes

  --link: https://www.youtube.com/watch?v=iTZ1-85I77c --


Inference:
-----------

- TS dont allow changing var/lets types later on
- Type is decided at the time of declaration
- Can declare a mixed array and that can then extend to the types used at the 
time of declaration
- Objects can get overridden if the schema is exactly same, but cannot 
add/reduce/update keys

Explicit Types:
----------------

- Define a type for a var at the time of declaration like 
    `
        let character: string;
        let users: string[] = [];
    `
- Can also initialize at the time of declaration like
    `let users: string[] = [];`
- Can define multiple types by unions like
    `let mixed: (string|number)[] = [];`
- Union of basic dataTypes dont require `()` like
    `let uid: number|string;`
- Explicit Types in objects can be achieved through 
        
        let userOne: object;
        let userTwo: {
          name: string,
          age: number,
          beltColor: string,
        };

Any (Dynamic) Type:
-------------------

- Can use `any` type which allows to then 
use any of the types
- It basically reverts what the TS is all about
- Works like `let uid: any = 101;`
- For arrays and objects, its like 
    
        let mixed: any[] = [];
        let user: { name: any, age: any };
     
WF and TSConfig:
----------------

- Better to place `index.html`, `styless.css`, and 
other files which are to be hosted publically in
`public` folder and all the source code and ts files in `src`
- Also, use the command `tsc --init` to generate tsConfig file
- For start conversion of TS - JS, change following in 
`tsconfig.json` file

        ...,
        "outDir": "./public",
        "rootDir": "./src",
        ...
- To only allow conversion of the files in `src` folder, add `include` 
in `tsconfig.json` like

        ...,
        "forceConsistentCasingInFileNames": true
      },
      "include": ["src"]

Functions:
----------

- Can define the functions and specify the type of each param
like

        const add = (a: number, b: number) => {

- For an optional parameter, can use either of the two ways

        const add = (a: number, b: number, c?: number | string) => {
        
        const add = (a: number, b: number, c: number | string = 20) => {
- The type of the function is inferred by the type of data it returns. 
 For a function which is'nt returning anything, the type would be `void` 
- Try declaring the optional params at the end after the required ones

type (Aliasing):
----------------

- Instead of repeating the types in unions, we can create `type` like

        type StringOrNum = string | number;
        type objWithName = { name: string, uid: StringOrNum };
        
        const greeting = (user: objWithName) => {...}

Function Signatures:
--------------------

- Can create signatures of functions and define the params, their types
and the function's return type as well like

        let calc: (a: number, b: number, c: string) => number;
        calc = (numOne: number, numTwo: number, action: string) => {
          if (action === 'add') {
            return numOne + numTwo;
          } else {
            return numTwo - numOne; // if we dont return in else, will throw error as its in signature that it should always return number
          }
        };

DOM:
----

- Can access DOM as in JS/React etc
- One difference is that during development, TS dont have access to
index.html and its element so it throws errors while accessing those
- There are a couple of solutions to cater that, if we are sure that the
element we are trying to access is in the DOM, we can use `!` like

        const anchorTag = document.querySelector('a')!;
        console.log(anchorTag.href);

Alternatively, we can use conditional statements or optional chaining like

        const anchorTag = document.querySelector('a');
        console.log(anchorTag?.href);

Type Casting:
-------------

- Even though, TS grabs the DOM elements, it doesnt know which type 
of DOM element it is. It just know which DOM element it is like an input, select, anchor etc
- To deal with it, we can use type casting like

        const anchorTag = document.querySelector('#anchorTag') as HTMLAnchorElement;

Classes:
--------

- Can define classes similar to JS
- All the properties of the class must hae defined types and 
needs to be initialized, can be initialized in the constructor like

        class Invoice {
          client: string;
          details: string;
          amount: number;
        
          constructor(client: string, details: string, amount: number) {
            this.client = client;
            this.details = details;
            this.amount = amount;
          }
        
          format = () => `${this.client} owes $${this.amount} for ${this.details}`;
        }
- Can create instances of class like

        const invoiceOne = new Invoice('Ema', 'work on the website', 500);      
- Can infer new variables/arrays with the type of this class like

        let invoices: Invoice[] = []; 

Access Modifiers:
-----------------

- The properties of the class can be `public`, `private` or `readonly` like

        class Invoice {
          // public client: string; // can avoid public as by default its public
          client: string;
          details: string;
          amount: number;
          readonly id: number;
          private identity: string;
        
          constructor(client: string, details: string, amount: number, id: number = 101, identity: string = 'secret') {
            this.client = client;
            this.details = details;
            this.amount = amount;
            this.id = id;
            this.identity = identity;
          }
        
          format = () => `${this.client} owes $${this.amount} for ${this.details}`;
        }
- Public properties are both access-able and can be modified outside the class (but 
the type cannot be changed) like

        invoiceOne.client = 'Matt';
- ReadOnly properties are access-able outside the class but we cant modify them
neither inside nor outside the class
- Private properties cannot be accessed outside the class
- if using access modifiers with all the params, we can use shorthand like

        class InvoiceUpdated {
          constructor(
            public client: string,
            readonly id: number = 101,
            private identity: string = 'secret'
          ) {
            this.client = client;
            this.id = id;
            this.identity = identity;
          }
        }
