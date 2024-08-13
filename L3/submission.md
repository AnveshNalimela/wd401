
# Choosing the Right Compile to JS Language for Your First Development Task

Compile to javascript languages are  imporatnt bcoz they provide developer a choice to write the code in their selected language and compile it to Javascript later.
It provides a enhanced developer experince and easily detection of error during the development itself through the static typing provided Typescript which is complile to javascript language



## Comparative analysis of TypeScript and Babel:





| TypeScript | Babel     |                
| :-------- | :------- |  
| TypeScript is a statically typed superset of JavaScript | Babel is a JavaScript compiler that allows developers to use the latest ECMAScript features  | 
| TS offers Type Checking to avoid type error during development| Babel doesnot offer the type checking|
|TS is higly recommended for large scale appliactions| Babel was recommnended for the modern applications|
|TS provides  Type annotations, type inference, interfaces, enums, generics,decorators | Babel provides Transpiling modern JavaScript to ES5, supporting latest JavaScript features,presets and plugins |
| TS compilation is slow due is type checking | whereas Babel compilation is faster |
| Type Safety,large Community |Versatility ,Ecosystem |






                      



## Type system and its advantages

A type system is a set of rules that a programming language uses to ensure that data is used correctly and consistently throughout the codebase.

TypeScript type system is a powerful tool that can help you write more reliable and maintainable code. By using the built-in types and defining your own types using type aliases and interfaces, you can catch errors and make your code easier to understand.
type system supports the Primitive Types which are like string,number,boolean etc ..
```typescript
let name: string = "hello";
let age:number=12;
let married:boolean= false;
```

In Typescript own customizable data types are defined using the interfaces 

```typescript
interface User {
  name: string;
  id: number;
  married: boolean;
}
```
Typescript type system enusres the type safety for variables,functions and annotations through type checking

```typescript
let name: string = "hello";
name = 42; // throws an error 
name = "false"; // No error 
```
### Advantages  of type system

- Easy Error Detection during the time of development itself which avoids the run time errors
- code becomes Self describing regarding the type annoataions and interfaces
- Better developer experince 
- provides standard way of definining the customozed data objects

### scernarios 

In case of capstone(sports scores and dashbaord) which is basically a large application which high number of files.
For such a large application it is recommended to use the typescript language over babel.
and we are not using highly using of ECMAScript features So there is no need  transpiling code to an older version of JavaScript that's widely supported.therfore we choose the typsecript over babel in the capstone project.


## Project Conversion

First of all our system need to install the typescript globally using the following command

```typescript
npm install -g typescript
```

Let setup the typescript configuartion in our project using the command 

```typescript
tsc --init
```
system generates the file named(tsconfig.json)
which looks simaliar to this(after the complete setup)

```json
{
  "compilerOptions": {
    "jsx": "react",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "module": "esnext",
    "allowJs": true,
    "skipLibCheck": true,
    "strict": false,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "noEmit": true, // or "emitDeclarationOnly": true,
    "allowImportingTsExtensions": true
  },
  "include": ["src", "vite-env.d.ts"]
}
```

Inorder to run the typescript files we need install then required dependencies
```js
npm install -D @types/node
```
```js
npm install -g ts-node
```

now rename the js files into ts files(with .ts extension)
and add type annotations to variables,functions

![image](https://github.com/user-attachments/assets/729cb9a8-ac06-45f2-b1f4-caa5c287a92c)



## Babel Configuration

```js
 babel({
      babelHelpers: 'bundled',
      exclude: 'node_modules/**',
      extensions: ['.js', '.jsx', '.ts', '.tsx'],
      presets: [
        '@babel/preset-env',
        '@babel/preset-react',
        '@babel/preset-typescript'
      ],
      plugins: [
        ['@babel/plugin-proposal-decorators', { legacy: true }],
        ['@babel/plugin-proposal-class-properties', { loose: true }]
      ]
    })
```
#### presets
- @babel/preset-env:Transpiles modern ES6+ JavaScript syntax to ES5
- @babel/preset-react:Transpiles JSX syntax to JavaScript.
- @babel/preset-typescript:Transpiles TypeScript code to JavaScript

#### plugins
- @babel/plugin-proposal-decorators:Adds support for the legacy decorator syntax
- @babel/plugin-proposal-class-properties:Adds support for the class properties syntax

## Case Study Presentation

Present a case study on choosing the appropriate compile-to-JS language for a project. Consider factors such as project size, team expertise, and future maintainability.

Presentation link:
https://docs.google.com/presentation/d/1cJRXidg-KQAYtP2yfODjn3hN8G-VUQCs4itfeXIQXWg/edit?usp=sharing


## Advanced TypeScript Features

TypeScript offers advanced features like decorators and generics that enhance the capabilities of the language, making it more powerful and flexible.

### Decorators
refer the following link for better understanding:https://www.typescriptlang.org/docs/handbook/decorators.html

Decorators are a feature in TypeScript that enable you to add metadata to classes, methods, properties, and parameters. They are widely used in frameworks like Angular for features like dependency injection and routing.

- Class Decorators: Used to modify or replace the constructor function of a class.
- Method Decorators: Alter the behavior of methods within a class.
- Property Decorators: Add metadata to class properties.
- Parameter Decorators: Affect the behavior of function parameters.
To enable experimental support for decorators, you must enable the experimentalDecorators compiler option either on the command line or in your tsconfig.json:




Command Line:
```typescript
tsc --target ES5 --experimentalDecorators
```
tsconfig.json:

```typescript

{
  "compilerOptions": {
    "jsx": "react",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "module": "esnext",
    "allowJs": true,
    "skipLibCheck": true,
    "strict": false,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "noEmit": true, // or "emitDeclarationOnly": true,
    "allowImportingTsExtensions": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  },
  "include": ["src", "vite-env.d.ts"]
}

```
First need to create the decorators and src/decorators folder in root directory of project
 we'll apply decorators using higher-order functions (HOFs) to wrap our methods.
 (src/decorators/log.ts)
 ```typescript
 export function withLog(fn: Function) {
  return function (...args: any[]) {
    console.log(`Function called with arguments: ${JSON.stringify(args)}`);
    const result = fn(...args);
    console.log(`Function returned: ${JSON.stringify(result)}`);
    return result;
  };
}

 ```
 (src/decorators/handleSubmit.ts)
 ```typescript
 import { SubmitHandler } from "react-hook-form";

export function withHandleSubmit<Inputs>(fn: SubmitHandler<Inputs>) {
  return function (data: Inputs) {
    console.log(`Form submitted with data: ${JSON.stringify(data)}`);
    return fn(data);
  };
}
 ```

 src/pages/signin/SigninForm.tsx)
 ```typescript
 import React, { useState } from "react";
import { SubmitHandler, useForm } from "react-hook-form";
import { useNavigate } from "react-router-dom";
import { API_ENDPOINT } from "../../config/constants";
import { withHandleSubmit } from "../../decorators/handleSubmit";
import { withLog } from "../../decorators/log";

interface Inputs {
  email: string;
  password: string;
}

const SigninForm: React.FC = () => {
  const navigate = useNavigate();
  const [msg, setMsg] = useState("");
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<Inputs>();

  const onSubmit: SubmitHandler<Inputs> = async (data) => {
    const { email, password } = data;

    try {
      const response = await fetch(`${API_ENDPOINT}/users/sign_in`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ email, password }),
      });

      if (!response.ok) {
        throw new Error("Sign-in failed");
      }

      console.log("Sign-in successful");
      const responseData = await response.json();

      localStorage.setItem("authToken", responseData.auth_token);
      localStorage.setItem("userData", JSON.stringify(responseData.user));
      navigate("/account");
    } catch (error) {
      console.error("Sign-in failed:", error);
      setMsg("Sign-in failed: Invalid username or password");
    }
  };

  return (
    <form onSubmit={handleSubmit(withHandleSubmit(withLog(onSubmit)))}>
      <div className="text-red-500 text-sm font-medium text-center my-3">
        {msg}
      </div>
      <div>
        <label className="block text-gray-700 font-semibold mb-2">Email:</label>
        <input
          type="email"
          id="email"
          {...register("email", { required: true })}
          className={`w-full border rounded-md py-2 px-3 my-4 text-gray-700 leading-tight focus:outline-none focus:border-blue-500 focus:shadow-outline-blue ${
            errors.email ? "border-red-500" : ""
          }`}
        />
      </div>
      <div>
        <label className="block text-gray-700 font-semibold mb-2">
          Password:
        </label>
        <input
          type="password"
          id="password"
          {...register("password", { required: true })}
          className={`w-full border rounded-md py-2 px-3 my-4 text-gray-700 leading-tight focus:outline-none focus:border-blue-500 focus:shadow-outline-blue ${
            errors.password ? "border-red-500" : ""
          }`}
        />
      </div>
      <button
        type="submit"
        className="w-full bg-gray-700 hover:bg-gray-800 text-white font-semibold py-2 px-4 rounded-md focus:outline-none focus:shadow-outline-gray mt-4"
      >
        Sign In
      </button>
      <a
        href="/signup"
        className=" mt-6 text-black-500  hover:underline hover:text-red-400"
      >
        Don't have an account? Create an account.
      </a>
    </form>
  );
};

export default SigninForm;

 ```

###  Generics

Generics in TypeScript allow you to create flexible and reusable components, functions, and classes by providing type parameters.

- Type Parameter Syntax: Generics use angle brackets (<T>) to define type parameters.
- Type Inference: TypeScript's type inference system helps determine the correct type when generics are used.
- Generic Constraints: You can specify constraints to restrict the types that can be used with generics.

have a look at code snippet provided which implemnents the generics 
```typescript
// src/context/projects/context.tsx

import React, { createContext, useContext, useReducer } from "react";
import { ArticleActions, ArticleState, initialState, reducer } from "./reducer";
const ArticleStateContext = createContext<ArticleState | undefined>(undefined);

// Lets define a new type called ProjectsDispatch using TypeScript.

type ArticleDispatch = React.Dispatch<ArticleActions>;

const ArticleDispatchContext = createContext<ArticleDispatch | undefined>(
  undefined
);
export const ArticleProvider: React.FC<React.PropsWithChildren> = ({
  children,
}) => {
  const [state, dispatch] = useReducer(reducer, initialState);

  // Next, I'll pass the `dispatch` object as value of this ProjectsDispatchContext.

  return (
    <ArticleStateContext.Provider value={state}>
      <ArticleDispatchContext.Provider value={dispatch}>
        {children}
      </ArticleDispatchContext.Provider>
    </ArticleStateContext.Provider>
  );
};

export const useArticleState = () => useContext(ArticleStateContext);

export const useArticleDispatch = () => useContext(ArticleDispatchContext);

```

## Best pratices for larger projects

- **Type safety** Use TypeScript's strong typing to find errors before running your code.

- **Modularization** Break your code into smaller, reusable parts to make it easier to manage and maintain.
- **Code Documentation** Write clear descriptions for your functions and classes to help others understand and work with your code
- **Testing** Create thorough tests for each part of your code to make sure everything works as expected.

