# **Webpack - Concept**

## **Understanding the Concepts of Webpack**

### **Getting Started**

1. **Initialize the Project**

   - Run `npm init -y` to create a `package.json` file with default settings.
   - Set `"private": true` in `package.json` to prevent accidentally publishing the package to the npm registry.

2. **Install Webpack**

   - Install Webpack and its CLI tool by running: 
     ```
     npm install webpack webpack-cli --save-dev
     ```
   - Webpack is a module bundler that helps to package all of your code and its dependencies into a single output file.

3. **Create a Simple JavaScript File**

   - Create a simple JavaScript file (`index.js`) that uses Lodash to demonstrate bundling.
   - Initially, add Lodash as a `<script>` tag in the `index.html` file so it can be used by your JavaScript code.

4. **Manual HTML File Creation**

   - For this initial setup, manually create the HTML file (`index.html`) in the `dist` folder. This will act as the entry point of your application.
   - In `index.js`, create a `div` element and append it to the DOM using the code:
     ```js
     function component() {
       const element = document.createElement('div');
       element.innerHTML = _.join(['Hello', 'Webpack'], ' ');
       return element;
     }
     document.body.appendChild(component());
     ```

5. **Bundling with Webpack**

   - Install Lodash as a project dependency:
     ```
     npm install lodash
     ```
   - Update `index.js` to import Lodash directly:
     ```js
     import _ from 'lodash';

     function component() {
       const element = document.createElement('div');
       element.innerHTML = _.join(['Hello', 'Webpack'], ' ');
       return element;
     }
     document.body.appendChild(component());
     ```
   - Remove the `<script>` tag for Lodash from `index.html`, as Lodash will now be bundled by Webpack.

6. **Output to Distribution Folder**

   - Webpack will bundle the output into the `dist` folder as `main.js`.
   - Make sure to reference `main.js` in your `index.html` file:
     ```html
     <script src="main.js"></script>
     ```
   - This allows the bundled JavaScript code to be loaded when the HTML file is opened.

7. **Running the Build**

   - To build the project, use the command:
     ```
     npx webpack
     ```
   - Since we have a Webpack configuration file (`webpack.config.js`), we can also run the build using:
     ```
     npx webpack --config webpack.config.js
     ```

   - This will generate the bundled file (`main.js`) in the `dist` folder.

### **Project Structure**

The project structure will look like this:

```
webpack-demo
|- package.json
|- package-lock.json
|- webpack.config.js
|- /dist
   |- main.js
   |- index.html
|- /src
   |- index.js
|- /node_modules
```

