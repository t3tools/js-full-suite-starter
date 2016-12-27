##Starter Kit
living, automated, and interactive checklist

+ Package management
+ Bundling
+ Minification
+ Sourcemaps
+ Transpiling
+ Dynamic HTML Generation
+ Centralized HTTP
+ Mock API Framework
+ Development Webserver
+ Linting
+ Automated Testing
+ Continuous Integration
+ Automated Build
+ Automated Deployment
+ Working Example Application



### `.editorconfig`
+ automated consistency

```
root = true

[*]
indent_style = space
indent_size = 3
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
indent_size = 2
```

### package manager: *npm*
- npm allows for build-step (linting, transpiling, minifying)
- npm package manifest: `package.json`
- security vulnerabilities: 
  - `npm install -g nsp` 
  - $: `nsp check`


### dev webserver
- express
- sharring devserver with **localtunnel**
  + `npm install -g localtunnel`
  + $: `lt --port 3000`
- deploying
  + static files: **surge**
    + `npm install -g now`
    + $: `surge`
  + node projects: **now**
    + `npm install -g now`
    + $: `now`
    
    
### automation: *npm*
  + use tools directly
  + no need for separate plugins
  ```
  scripts: {
    "start": "npm-run-all --parallel security-check share open:src",
    "security-check" : "nsp check",
    "share" : "lt --port 3000",
    "open:src" : "node ./build-scripts/src-server.js"
  } 
  ```
  
### transpilation: *babel*
babel, typescript, elm
+ easier to read
+ no waiting for tras 


### bundling: *webpack*
+ commonjs doesn't work in web browsers
+ package project into files
+ improve node perf
+ should use es6 modules (named imports, default exports )
  + statically analyzable
+ webpack
  + bundles css, images, fonts, html
  + bundle splitting 
  + hot module reloading
  + creates source maps

(1)`webpack.config.dev.js`
```
import path from 'path';

export default {
   debug: true,
   devtool: 'inline-source-map',
   noInfo: true,
   entry: [
      path.resolve(__dirname, 'src/index')
   ],
   target: 'web',
   output: {
      path: path.resolve(__dirname, 'src'),
      publicPath: '/',
      filename: 'bundle.js'
   },
   plugins: [],
   module: {
      loaders: [
         {test: /\.js$/, exclude: /node_modules/, loaders: ['babel']},
         {test: /\.css$/, loaders: ['style', 'css']}
      ]
   }
}
```

### linting: *ESLint*
+ Enforce consistency
  - Curly brace position
  - trailing commas
  - global vars

+ Avoid mistakes
  - adding parens
  - overwriting funcs
  - assignments in conditional
  - missing default case in switch
  - debugger / console.log

JSLint --> JSHint --> ESLint (current standard)

+ Linter decisions
  - config format?
    + package.json v .eslintrc
  - which lint rules?
    + 
  - warnings or errors? 
    + warnings allow continue dev; error breaks the build
  - plugins? 
    + enhance for react/angular/node?
  - use preset instead?
    + recommended-default standard vs. airbnb vs. xo 

####(1) Configuration : `eslint.json`
``` 
{
  "root" : true,
  "extends" : [
    "eslint:recommended",
    "plugin:import/errors",
    "plugin:import/warnings"
  ],
  "parserOptions" : {
    "ecmaVersion" : 7,
    "sourceType" : "module"
  },
  "env" : {
    "browser" : true,
    "node" : true,
    "mocha" : true
  },
  "rules" : {
    
  }
}
```

####(2) Watching w/ eslint-watch
in `package.json` scripts:     
```
"lint" : "esw webpack.config.* src buildScripts"
```

### Testing
1. Unit Testing (mocha, jasmine)
  + tests a small unit of code
  + run upon save
2. Integration : (nightwatch)
  + tests units of code that interact (browser interactions, api reqs, etc)
  + run on demand
3. Automated UI : (selenium)

Testing Decisions

| | | |
|-------|-----|-----|
| Testing framework | **mocha** |  
| Assertion Library | **chai** |
| Browser Library | **Karma** |    
| DOM simulation Library | **jsDOM**| 
| Headless Browser Library | **PhantomJS**| 
| Testing engine | **node** |  


Recommended
- Write tests in same files as source-code
- Unit tests should be automated on save

#### Continuous Integration
CI Server Options: 
+ Travis (linux, hosted)
+ Jenkins (linux, local)
+ Appveyor (windows)

Why?

- Run automated build
- Run your tests
- Check code coverage
- Automate deployment

#### (1)Configure `.travis.yml` 
```
language: node_js
node_js: 
  - "6"
```

### HTTP Calls
- Node : http, request
- Browser : XMLHttpRequest, jQuery, Fetch
- Node + Browser : isomorphic-fetch,  xhr, Axios

####Centralizing API Calls
- Configure all calls
- Handle preloader logic
- Handle errors
- Single seam for mocking
