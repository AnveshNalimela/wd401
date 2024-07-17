
# Level M2: Optimizing JS Integration in Your First Web Development Task

## Webpack setup and Configuartion 

Webpack is a highly configurable tool, allowing you to tailor the bundling process to your specific project requirements. Proper configuration is crucial for optimizing asset management and minimizing load times.

Install the neccesary depencies using the npm

```bash
npm install webpack webpack-cli webpack-dev-server html-webpack-plugin clean-webpack-plugin css-loader style-loader
```
The project includes the typescript hence ts-loader is installed

```bash
npm install typescript @types/react @types/react-dom ts-loader
```

## WebPack Configuration 
webpack configuartion of our project looks like:
we handled the tsx and css files avaliable in the project

```javascript
import { CleanWebpackPlugin } from 'clean-webpack-plugin';
import HtmlWebpackPlugin from 'html-webpack-plugin';
import path from 'path';
import { fileURLToPath } from 'url';
// Get __dirname equivalent in ES Modules
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);
export default {
    entry: './src/main.tsx',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
    },
    resolve: {
        extensions: ['.js', '.jsx', '.ts', '.tsx'],
    },
    module: {
        rules: [
            {
                test: /\.tsx?$/,
                use: 'ts-loader',
                exclude: /node_modules/,
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'],
            },
            {
                test: /\.(png|jpe?g|gif)$/i,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            name: '[name].[hash].[ext]',
                            outputPath: 'images', // Specifies the directory for images
                        },
                    }]
            }

        ],
    },
    plugins: [
        new CleanWebpackPlugin(),
        new HtmlWebpackPlugin({
            template: './public/index.html',
        }),
    ],
    devServer: {
        static: {
            directory: path.join(__dirname, 'public'),
        },
        compress: true,
        port: 3000,
    },
optimization: {
        splitChunks: {
            chunks: 'all',
        },
    },
};


```

We are using ES Modules not commonJs hence we have js files

## update the scripts in package.json files
```javascript
"scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint . --ext js,jsx,ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview",
    "webpack-dev": "webpack serve --mode development",
    "webpack-build": "webpack --mode production"
  },
```

![image](https://github.com/user-attachments/assets/ca2ee5f0-f4a8-407d-98eb-7ff32e390da4)

## customized configuartion for css and images

To efficiently handle CSS and images in your Webpack configuration, you can use loaders like style-loader, css-loader, and file-loader or url-loader. 

```bash
module: {
        rules: [
            {
                test: /\.tsx?$/,
                use: 'ts-loader',
                exclude: /node_modules/,
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader'],
            },
            {
                test: /\.(png|jpe?g|gif)$/i,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            name: '[name].[hash].[ext]',
                            outputPath: 'images', // Specifies the directory for images
                        },
                    }]
            }

        ],
    },
```
Here The style-loader and css-loader will process and inject CSS into your application, while file-loader (or asset/resource in Webpack 5) will handle importing image files.




# Advanced Bundling Techniques

## code splitting:
It helps in breaking down large bundles into smaller chunks, which can be loaded on demand, thus improving the performance and load time of your web application.

code snippet from the project where lazy loading is implemneted:

```javascript
 optimization: {
        splitChunks: {
            chunks: 'all',
        },
    },
```
The webpack connfiguartion in project includes the optimization.split chunks which tells webapck how to split chunks for optimization

## lazy loading:
load components only when they are required, such as when a user navigates to a specific route.

```javascript
import React, { Suspense } from "react";
import ErrorBoundary from "../../components/ErrorBoundary";
const ArticlesList = React.lazy(() => import("./articles/ArticlesList"));
const MatchList = React.lazy(() => import("./matches/MatchList"));

const Home = () => {
  return (
    <>
      <div className="flex justify-between text-center text-md-left px-5">
        <h2 className="text-2xl font-bold tracking-tight text-slate-700">
          Live Matches
        </h2>
      </div>
      <ErrorBoundary>
        <Suspense fallback={<div className="suspense-loading">Loading...</div>}>
          <MatchList />
        </Suspense>
      </ErrorBoundary>

      <div className="flex justify-between text-center text-md-left px-5">
        <h2 className=" text-2xl font-bold tracking-tight text-slate-700">
          Trending Articles
        </h2>
      </div>
      <div className="flex">
        <ErrorBoundary>
          <Suspense
            fallback={<div className="suspense-loading">Loading...</div>}
          >
            <ArticlesList />
          </Suspense>
        </ErrorBoundary>
      </div>
    </>
  );
};

export default Home;

```

## benefits of code splitting and lazy loading

- Reduced Initial Load Time: Only the necessary code for the initial page is loaded, reducing the time it takes for the page to become interactive.
- Optimized Resource Usage: Resources are loaded as needed, improving performance and reducing bandwidth usage.
- Better Caching: Since chunks are smaller and more specific, they can be cached more effectively. If only one part of the application changes, only the relevant chunks need to be reloaded.

## Introduction to Import Maps

Import maps are a feature in JavaScript that allows you to control how module specifiers are interpreted by the browser. They enable you to specify URLs for your modules directly in your HTML, without the need for a traditional build step or bundler like Webpack or Rollup.

```bash
<script type="importmap" src="import-map.json"></script>
```

## benefits of Import Maps

- simplification
- Performance
- Flexibility

## Refactoring the sample project using import maps 

Before refactoring(index.html):

```html
<!doctype html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="https://cdn-icons-png.flaticon.com/512/1099/1099680.png" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>SportBuzz</title>
</head>

<body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
</body>

</html>

```

and Main.tsx in src directoray

```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import { ThemeProvider } from "./context/theme";
import "./index.css";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <ThemeProvider>
    <App />
  </ThemeProvider>
);


```
## refactoring using import map
Create a import-map.json file in root directory
importmap.json file

```json
{
  "imports": {
    "/src/": "./src/",
    "react": "https://cdn.skypack.dev/react",
    "react-dom": "https://cdn.skypack.dev/react-dom",
    "./App": "./src/App",
    "./context/theme": "./src/context/theme",
    "./index.css": "./src/index.css"
  }
}

```

Now refactored index.hmtl using the import maps 

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="https://cdn-icons-png.flaticon.com/512/1099/1099680.png" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>SportBuzz</title>
    <script type="importmap" src="/importmap.json"></script>
</head>

<body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
</body>

</html>


```
and main.tsx looks like
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

// Used mapped imports from import-map.json
import { ThemeProvider } from 'context/theme';
import './index.css';
import App from 'App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <ThemeProvider>
    <App />
  </ThemeProvider>
);

```


Issue related to import maps

- Modern browsers support the import map whereas some browsers (older versions) wont support the import maps hence it is nesscary to check before deploying

- import maps are static in nature and laoded during the html page load whenever there is change entire page need to reloaded

- security issues regrading the URLs
