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

Certainly! I will now provide the detailed content for the **Development Mode** section, following the same format you provided earlier. I'll expand each point with examples, configurations, and additional information.

---


### **Development Mode in Webpack**

Webpack provides a development mode to make your coding process more efficient. In this mode, several optimizations and tools are available to enhance the speed and flexibility of the development process. The main focus is on enabling tools like **source maps**, **watch mode**, **live reloading**, **hot module replacement**, and **optimizing shared dependencies** across multiple entry points.

---

### 1. **Environment Variables for Better Development Workflow**

Webpack uses the `mode` option to distinguish between development and production environments. Setting the environment to **development** configures Webpack to:
- Enable useful debugging features (e.g., source maps).
- Minimize the time spent on bundling by skipping minification and other optimizations.
- Provide better error messages and detailed logs.

#### Configuration:
In your `webpack.config.js`, set the mode to `"development"`:

```javascript
module.exports = {
  mode: 'development', // Set to development mode
  // Other configurations
};
```

Alternatively, you can specify the mode using an environment variable in the command line:

```bash
webpack --mode development
```

#### Key Benefits in Development Mode:
- Fast build times due to fewer optimizations.
- Detailed logging and error messages to help debug code.
- Source maps enabled by default for easier debugging.

---

### 2. **Source Maps to Trace Errors to the Original Source Code**

In development mode, **source maps** are essential for tracing errors and debugging the original source code (before it’s bundled). Source maps map the minified/compiled code back to the original files.

#### Configuration:
Add the `devtool` option in your `webpack.config.js` to enable source maps. For development, use the `"eval-source-map"` option for the fastest rebuilds:

```javascript
module.exports = {
  mode: 'development',
  devtool: 'eval-source-map', // Enable source maps for easier debugging
  // Other configurations
};
```

#### Explanation of `devtool`:
- **`eval-source-map`**: Fastest source map option for development. Maps the source files accurately and shows line numbers in the console.

#### Why You Don’t Need Source Maps in Production:
Source maps make it easier for developers to debug their code, but they expose the original code structure. In production, disabling or minimizing source maps is a good security practice.

---

### 3. **Webpack Watch Mode for Automatic Rebuilding**

Webpack’s **watch mode** watches your files for changes and automatically rebuilds the bundle whenever a file is saved. This reduces the need for manually running build commands.

#### How to Use Watch Mode:
In the command line, run:

```bash
webpack --watch
```

#### Configuration in `webpack.config.js`:
```javascript
module.exports = {
  watch: true, // Enable watch mode in the config
  // Other configurations
};
```

#### Problem with Watch Mode:
While **watch mode** can automatically rebuild your files, it doesn’t automatically refresh the browser. You’ll still need to refresh the page manually to see the updated bundle.

---

### 4. **Webpack Dev Server and Hot Module Replacement (HMR)**

To solve the issue of having to manually refresh the browser after each rebuild, Webpack provides the **Webpack Dev Server** with **Hot Module Replacement (HMR)**. This setup reloads the page automatically and can even replace only the changed modules without refreshing the entire page.

#### Installation:
Install the Webpack Dev Server as a development dependency:

```bash
npm install webpack-dev-server --save-dev
```

#### Configuration for Webpack Dev Server:
In `webpack.config.js`, configure the dev server and enable HMR:

```javascript
module.exports = {
  mode: 'development',
  devServer: {
    contentBase: './dist',  // Serve files from the dist folder
    hot: true,              // Enable hot module replacement (HMR)
  },
  // Other configurations
};
```

#### Running the Dev Server:
Use the following command to run the dev server with hot reloading enabled:

```bash
npx webpack serve
```

#### Benefits of Webpack Dev Server with HMR:
- **Live Reloading**: Automatically reloads the browser when changes are made.
- **Hot Module Replacement (HMR)**: Replaces only the changed modules in the browser, keeping the application state intact (e.g., no page reload needed for UI changes).

**Example**:
If you're developing a React component, HMR will update the specific component in the browser without refreshing the entire page.

#### Pitfalls:
- Not all libraries fully support HMR. Some require additional setup (e.g., React with `react-hot-loader`).
- Be careful with stateful components; sometimes HMR can interfere with the expected state behavior.

---

### 5. **Handling Multiple Entry Points and Shared Dependencies**

When developing larger applications with multiple entry points, it’s common to have **shared dependencies** (e.g., libraries like React or Lodash). If these dependencies are bundled separately for each entry point, it leads to duplication and inefficiency.

#### Problem:
If you have multiple entry points in your application (for example, `main.js` and `admin.js`), and both require Lodash, Webpack will bundle Lodash separately for each entry point by default, leading to duplicate dependencies in the final bundles.

#### Solution:
To avoid duplicate dependencies, you can configure Webpack’s **optimization** option to extract the runtime code and shared dependencies into a single chunk.

#### Configuration:
In your `webpack.config.js`, use the `runtimeChunk: 'single'` option to extract runtime dependencies:

```javascript
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
    admin: './src/admin.js',
  },
  optimization: {
    runtimeChunk: 'single', // Ensures only one instance of shared dependencies
  },
  // Other configurations
};
```

This creates a separate runtime file that both entry points can share, avoiding duplication.

---

### 6. **Code Splitting in Development Mode**

Webpack **splits the code** based on three things:
1. **Entry Points**: Each entry point specified in the configuration can be split into its own bundle.
2. **Dynamic Imports**: When you use `import()` in your code, Webpack can automatically split the bundle and load parts of the code on-demand.
3. **Using the SplitChunksPlugin**: This plugin helps you extract common dependencies (like third-party libraries) into separate bundles.

#### Configuration for SplitChunks:

```javascript
module.exports = {
  mode: 'development',
  optimization: {
    splitChunks: {
      chunks: 'all', // Applies to all chunks (both dynamic and entry chunks)
    },
  },
  // Other configurations
};
```

By enabling `splitChunks`, Webpack will automatically extract common dependencies into a separate file, reducing the size of individual bundles.

---

### 7. **Handling Shared Dependencies Across Multiple Entry Points**

If two entry points depend on a shared dependency, Webpack allows you to specify that dependency in the configuration. For example, if both `main.js` and `admin.js` use Lodash, Webpack can create a shared dependency that is loaded once and used by both entry points.

#### Configuration Example:
In `webpack.config.js`:

```javascript
module.exports = {
  mode: 'development',
  entry: {
    main: './src/index.js',
    admin: './src/admin.js',
  },
  optimization: {
    runtimeChunk: 'single', // Extract runtime for both entry points
    splitChunks: {
      cacheGroups: {
        commons: {
          test: /[\\/]node_modules[\\/]/, // Shared dependencies from node_modules
          name: 'vendors', // Name the shared chunk as vendors.js
          chunks: 'all', // Apply to all entry points
        },
      },
    },
  },
};
```

#### Benefits:
- **Efficiency**: Shared dependencies (like React or Lodash) are only bundled once, even if used by multiple entry points.
- **Smaller Bundles**: Each entry point bundle is smaller and faster to load.

---

### 8. **SplitChunksPlugin to Extract Common Dependencies**

Webpack’s `SplitChunksPlugin` allows you to extract common dependencies into an existing entry chunk or a new chunk. This helps avoid duplication when multiple entry points share the same dependencies.

#### Configuration for `SplitChunksPlugin`:

```javascript
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all', // Apply to both async and non-async chunks
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/, // Split out dependencies from node_modules
          name: 'vendors',
          chunks: 'all',
        },
      },
    },
  },
};
```

#### Example:
If you have `React` used across multiple entry points, the `SplitChunksPlugin` will extract `React` into its own `vendors.js` file. This way, React is downloaded once and shared across all the entry points.

---

## **Summary**

Webpack development mode is designed to make your workflow faster and more efficient. Key takeaways include:
- Use **environment variables** to differentiate between development and production modes.
- Enable **source maps** for easier debugging.
- Utilize **watch mode** for automatic rebuilding.
- Leverage **Webpack Dev Server and HMR** for live reloading and hot module replacement.
- Optimize **shared dependencies** across multiple entry points using `runtimeChunk` and `SplitChunksPlugin`.

