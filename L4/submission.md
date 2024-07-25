### Adding tests to the project

## Configuration of Testing Framework
I have configured the jest and cypress and vitest testing framework for my project 
My project (sportBuzz) is an react-based application devloped using the typescript compile to js language supported ESM modules not common.js
I have cofigured the jest from unit testing and cypress for integration testing.
## Jest configuartion
I have configured jest in project 
Installing the necessary dependencies through npm :

```javascript
npm install --save-dev jest ts-jest @types/jest @testing-library/react @testing-library/jest-dom @testing-library/user-event

```
and configure the jest.config.js file in the roor folder
```javascript
export default {
    // A list of paths to directories that Jest should use to search for files in
    // https://jestjs.io/docs/configuration#roots-arraystring
    roots: ["<rootDir>/src/"],

    // The test environment that will be used for testing, jsdom for browser environment
    // https://jestjs.io/docs/configuration#testenvironment-string
    testEnvironment: "jsdom",

    // Jest transformations
    // https://jestjs.io/docs/configuration#transform-objectstring-pathtotransformer--pathtotransformer-object
    transform: {
        "^.+\\.tsx?$": "ts-jest", // Transform TypeScript files using ts-jest
    },

    // A list of paths to modules that run some code to configure or set up the testing framework before each test file in the suite is executed
    // https://jestjs.io/docs/configuration#setupfilesafterenv-array
    setupFilesAfterEnv: ["<rootDir>/jest.setup.ts"],

    // Code coverage config
    // https://jestjs.io/docs/configuration#collectcoveragefrom-array
    coverageDirectory: "<rootDir>/coverage/",
    collectCoverageFrom: [
        "<rootDir>/src/**/*.{ts,tsx}",
        "!**/__mocks__/**",
        "!**/node_modules/**",
        "!**/*.d.ts",
    ],

    // Important: order matters, specific rules should be defined first
    // https://jestjs.io/fr/docs/configuration#modulenamemapper-objectstring-string--arraystring
    moduleNameMapper: {
        // Handle CSS imports (with CSS modules)
        // https://jestjs.io/docs/webpack#mocking-css-modules
        "^.+\\.module\\.(css|sass|scss|less)$": "identity-obj-proxy",

        // Handle CSS imports (without CSS modules)
        "^.+\\.(css|sass|scss|less)$": "<rootDir>/__mocks__/styleMock.js",

        // Handle static assets
        // https://jestjs.io/docs/webpack#handling-static-assets
        "^.+\\.(jpg|jpeg|png|gif|webp|avif|svg|ttf|woff|woff2)$": `<rootDir>/__mocks__/fileMock.js`,

        // Handle TypeScript path aliases
        "^@/(.*)$": "<rootDir>/src/$1",
    },

    verbose: true,
    testTimeout: 30000,
};

```

Setup configuaration for the jest.setup.ts

```typescript
import "@testing-library/jest-dom";

```

later update the tsconfig.json and add the scripts for the unit testing using jest :
```json
 "include": [
    "src",
    "vite-env.d.ts",
    "jest.setup.ts",
    "custom.d.ts",
    "cypress/**/*.ts"
  ]
```

and scripts in package.json
```json
  "jest:test:watch": "jest --watch",
    "jest:test": "jest",
    "jest:test:coverage": "jest --silent --watchAll=false --coverage",
```

Create a  __tests__ folder in src abd create a file with action.test.tsx and add few testcases
abd run the test cases
![image](https://github.com/user-attachments/assets/0eae10f7-7572-4df3-921e-d1c376f912ff)


But There is a problem there is file (src/config/constants.ts)
```typescript
export const API_ENDPOINT = import.meta.env.VITE_API_ENDPOINT;
// export const API_ENDPOINT = process.env.VITE_API_ENDPOINT||"" ;

```

Actually the jest frame work support the process.env but import.meta.env is suporting in mystem so i used vitest incase of jest for github workflow

Setup of vitest is similar to jest.Scripts to run the vitest unit testing are  are :

![image](https://github.com/user-attachments/assets/a7dca186-12ed-43b3-858c-3ee5d616160f)

 and the results of test suite are also simaliar
 
 ![image](https://github.com/user-attachments/assets/107c5b0d-f29b-4f38-97b4-d5f25fce81dc)

 ## Cypress testing framwork configuartion and setup:
 Install the cypress in your project using npm (or else gloabally based on your requirment)
 ```javascript
npm install cypress --save-dev
 ```
Above command install the cypress in your project and avaliable in node modules folder
 ```js
npx cypress open
```
Above command creates a folder in project named cypress to store the testing files and cypress.config.json file in root directory
![image](https://github.com/user-attachments/assets/53eb80fb-7492-41c9-a91d-2b9903eaa931)




Before going to testing we need to configure the cypress in the project by updating the cypress.config.json

```json
import { defineConfig } from "cypress";

export default defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    baseUrl: "http://localhost:5173/",
  },
  env: {
    API_ENDPOINT: "https://wd301-capstone-api.pupilfirst.school", // or your actual API endpoint
  },
});

```
We have to add the scripts in package.json to test with the cypress
```json
 "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint . --ext js,jsx,ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview",
    "webpack-dev": "webpack serve --mode development",
    "webpack-build": "webpack --mode production",
    "cypress:test": "npx cypress run",
    "cypress:open": "npx cypress open",
    "jest:test:watch": "jest --watch",
    "jest:test": "jest",
    "jest:test:coverage": "jest --silent --watchAll=false --coverage",
    "vitest:test": "vitest",
    "vitest:test:ci": "vitest run"
  },
```
we have to run the script commad to run the cypress through the terminal
```js
npm run cypress:open
```
its open the window like this andd select the apporiate testing  option 
![image](https://github.com/user-attachments/assets/6a5fa547-f53f-4278-a8b7-43633e5722ec)
and then select the browser 
![image](https://github.com/user-attachments/assets/2af9613b-6496-4af8-9a46-995096a7c41e)
Add the new with .cy.ts extension and write the test suite for various components
and ensure that application is running on the baseurl mentioned in selected browser
![image](https://github.com/user-attachments/assets/d8bbd663-ccd7-46d0-aff7-1675df3f382a)


## Test Suite and Code Coverage:
Code coverage respresnets the how much code get executed during the execution of progarm. A high quality code base would have a code coverage above 80 to 90% .
it helps to know the quality of code and identify the code which is dead and not being used 
Generally the applications are deployed in produaction until they are tested properly
testing and code coverage goes hand in hand 
We can cover the entire application with test suite under __tests__ folder in src

Jest for testing.(unit testing)
React Testing Library for DOM testing.
Cypress for end-to-end tests (integraion testing).



## Automatic Test Suite Execution on GitHub

After comppletion of writing the unit and integraion test in your project 
Inorder to automatic Test Suite Execution on GitHub we use the github workflows concept
GitHub Actions is a powerful tool for automating software workflows, including running tests, building applications, and deploying code.
#### create workflow file :
GitHub Actions workflows are defined in YAML files located in the .github/workflows directory of your repository.
#### Define the Workflow: 
Specify the jobs and steps required to run your tests include steps for installing dependencies, running unit and integration tests, and running end-to-end tests with Cypress
#### push changes to github 
whenever the push files to github then github action checks for the (./github/workflows) and execute the files with extension(.yml) inside the folder
the step icludes the test,buld and deploy procedure 

```yml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Use Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "20"

      - name: Clean install dependencies
        run: |
          rm -rf node_modules
          rm -f package-lock.json
          npm install

      - name: Run Vitest(unit) tests
        run: npm run vitest:test:ci

      - name: Run Cypress(integration) tests
        uses: cypress-io/github-action@v5
        with:
          build: npm run build
          start: npm run dev
          wait-on: "http://localhost:5173"

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'

    steps:
      - uses: actions/checkout@v3

      - name: Use Node.js
        uses: actions/setup-node@v3
        with:
          node-version: "20"

      - name: Clean install dependencies
        run: |
          rm -rf node_modules
          rm -f package-lock.json
          npm install

      - name: Build(build the application)
        run: npm run build

      - name: Start dev server(start the application in devlopement server)
        run: |
          npm run dev &
          echo $! > ./dev-server.pid

      # Add deployment steps here if needed

      - name: Stop dev server
        run: kill $(cat ./dev-server.pid)

```
Above file is explained here

The event is trigged whenever there is push or push request made to 'main' branch
And the file includes two jobs mentioned one is test and other one is deploy
let start with test job:
The test job runs on the latest Ubuntu environment.
it first checks out the code from the repository using the actions/checkout@v3 action.
Sets up Node.js version 20 for the environment using the actions/setup-node@v3 action.
Removes any existing node_modules and package-lock.json to ensure a clean installation of dependencies by running npm install.
Runs unit tests using Vitest by executing npm run vitest:test:ci
Runs Cypress integration tests using the Cypress GitHub action. It:
- Builds the application with npm run build.
- Starts the development server with npm run dev.
- Waits for the application to be available at http://localhost:5173 before running the tests.

![image](https://github.com/user-attachments/assets/3418ae6b-e51f-4f09-89f2-fd4075cec604)

![image](https://github.com/user-attachments/assets/d40cdf78-b7ed-4821-b7d9-d94b1f6cbf4e)
 and deploy job : Runs only if the tests pass and the push is to the main branch. It builds and starts the application, preparing it for deployment.
![image](https://github.com/user-attachments/assets/cf960f4e-f1a1-4f68-aad1-38aa257e3416)


