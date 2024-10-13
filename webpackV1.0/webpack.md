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
       const element = document.createElement("div");
       element.innerHTML = _.join(["Hello", "Webpack"], " ");
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
     import _ from "lodash";

     function component() {
       const element = document.createElement("div");
       element.innerHTML = _.join(["Hello", "Webpack"], " ");
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

### **Asset Management**

So far, we have bundled a simple JavaScript file, but a project can contain assets other than JavaScript, such as CSS, images, and fonts. Webpack helps manage these assets effectively.

1. **Understanding Asset Management**

   - By default, Webpack only bundles JavaScript and JSON files without any additional configuration.
   - To bundle other types of files, we need to use **loaders**. Loaders enable Webpack to process and bundle non-JavaScript files, converting them into valid modules that can be used within your application.

2. **Bundling CSS Files**

   - To include CSS in your bundled output, you need to use `css-loader` and `style-loader`.
   - **css-loader**: Allows Webpack to process `@import` and `url()` in CSS files, resolving them as dependencies.
   - **style-loader**: Injects CSS into the DOM.
   - To set up CSS bundling, first install the necessary loaders:
     ```
     npm install css-loader style-loader --save-dev
     ```
   - Update `webpack.config.js` to include CSS handling:
     ```js
     module.exports = {
       module: {
         rules: [
           {
             test: /\.css$/i,
             use: ["style-loader", "css-loader"],
           },
         ],
       },
     };
     ```
   - Now, you can import a CSS file in your JavaScript:
     ```js
     import "./style.css";
     ```

3. **Handling Images and Fonts**

   - **Asset Modules**: Webpack's **Asset Modules** are built-in modules that can handle images, fonts, and other assets.
   - To load images, you can use the asset/resource module, which copies the file to the `dist` folder and includes the URL in your bundled output.
   - Example configuration for images:
     ```js
     module.exports = {
       module: {
         rules: [
           {
             test: /\.(png|svg|jpg|jpeg|gif)$/i,
             type: "asset/resource",
           },
         ],
       },
     };
     ```
   - This ensures that images referenced in your CSS or JavaScript are bundled correctly.
   - Similarly, fonts can also be handled using `asset/resource`:
     ```js
     module.exports = {
       module: {
         rules: [
           {
             test: /\.(woff|woff2|eot|ttf|otf)$/i,
             type: "asset/resource",
           },
         ],
       },
     };
     ```

4. **Loading Other Data Files**
   - Webpack can also be configured to load other types of data, such as CSV, XML, YAML, JSON5, and TOML files.
   - Install the necessary loaders:
     ```
     npm install csv-loader xml-loader yaml-loader json5-loader toml-loader --save-dev
     ```
   - Update `webpack.config.js` to include CSV, XML, YAML, JSON5, and TOML file handling:
     ```js
     module.exports = {
       module: {
         rules: [
           {
             test: /\.(csv|tsv)$/i,
             use: ["csv-loader"],
           },
           {
             test: /\.xml$/i,
             use: ["xml-loader"],
           },
           {
             test: /\.ya?ml$/i,
             use: ["yaml-loader"],
           },
           {
             test: /\.json5$/i,
             type: "json",
             parser: {
               parse: require("json5").parse,
             },
           },
           {
             test: /\.toml$/i,
             type: "json",
             parser: {
               parse: require("toml").parse,
             },
           },
         ],
       },
     };
     ```
   - This allows you to import and work with CSV, XML, YAML, JSON5, and TOML data within your JavaScript files.

### **Output Management**

1. **Improving Output Management**

   - So far, we have manually edited or added the files into `index.html`. However, Webpack can automate this process by using plugins.
   - To simplify managing the HTML file, we can use the `html-webpack-plugin` to automatically generate an `index.html` that includes references to the bundled output.

2. **Installing HTMLWebpackPlugin**

   - Install the HTML Webpack Plugin by running:
     ```
     npm install html-webpack-plugin --save-dev
     ```
   - Update `webpack.config.js` to include the plugin:

     ```js
     const HtmlWebpackPlugin = require('html-webpack-plugin');

     module.exports = {
       entry: './src/index.js',
       output: {
         filename: 'main.js',
         path: path.resolve(__dirname, 'dist'),
         clean: true, // Automatically clean the /dist folder before each build
       },
       plugins: [
         new HtmlWebpackPlugin({
           title: 'Webpack Demo',
           template: './src/index.html', // Use a template if needed
         }),
       ],
       module: {
         ...
       },
     };
     ```

3. **Cleaning the Output Folder**
   - With the `clean` option enabled in the output configuration, Webpack will automatically clean the `dist` folder before each build, ensuring that old files are removed.
   - This keeps the `dist` folder clean and makes sure that only the latest build files are present.
