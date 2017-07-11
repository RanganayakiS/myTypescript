This is an example/quickstart Eikon application written in Typescript. 
The idea of the application is to use the power of Typescript language while relying on Polymer and ELF for building UI.

It provides a basic backbone to base a typical Eikon application on:


HTML5 client application 
Sample Eikon App Engine Services
Webpack scripts for development and production builds


Also the sample provides some examples of typical tasks:


Custom components in conjunction with Eikon components
Wrapping of JET and JET plugins into services: JET Quotes, JET Archive, JET Settings
Retrieving data from Sample Eikon App Server service 
Retrieving ADC data via UDF
Typescript API description for JET, ELF and AppServer. This is still mostly incomplete and hopefully will be completed in future. 
Sample unit testing


The client UI application is build and bundled with Webpack into 3 main Javascript files (main.js, vendor.js, polyfill.js) and 
some additional files are copied directly as they are required by ELF and JET and expected to be loaded dynamically.

Eikon App Server services are transpiled with typescript without bundling.


Technology stack and main dependencies


Typescript 2.x
Polymer 1.x
Polymer-TS for wrapping Polymer code into typescript
InversifyJS as IoC container
Webpack 2.x to build an application
Eikon Element Library Framework components
Javascript Eikon Toolkit Library
Karma/Jasmine


Additional useful dependencies in this project include:


Bluebird as a Promise library
Momentjs for date formatting and potential internationalization
Numberjs for number formatting and potential internationalization



Prerequisite

Eikon App Engine provides Eikon Application Server functionality for local development

Before you start, please install project dependencies. bower-art-resolver is required to access internal libraries via bower.

npm install -g bower-art-resolver
npm i

Running

In order to run an application one should first build it and then publish this build into Eikon App Engine.

Development build, which results in non-minified files with sourcemaps in /__dist/

npm run dev
Production build, results in minified files in  /__dist/

npm run prod

Testing

Tests are implemented very roughly and are subject to some further development.
As for now unit testing is supported by running

npm test
You can also debug tests by running them in Chrome:

karma start karma.conf.js --browsers=Chrome --single-run=false
In initial version there's a single unit test for Jet Quote Service. 


TODOs for testing:


Add source-maps support for debugging
Add code-coverage
Consider adding e2e tests



Project structure key directories and files


/app.json App Server configuration file, includes application name, version and environment variables
/config/ webpack configuration files
/src/ application source-code directory
/src/vendor.ts for referencing 3rd party libraries and static resources which are not subject to change. This filed also includes some little quircks to make JET and UIFR works with bundled scripts.
/src/vendor-elements.ts for referencing ELF, Polymer project and 3rd party web components. Bundled into vendor.js
/src/polyfill.ts for potential polyfill libraries and code snippets
/src/main.ts main entry point script for an application, composition scope for IoC-container
/src/elements.html for referencing custom web components, bundled into main.js
/src/index.html application entry point
/src/api for Typescript API description of ELF, App Engine, JET and other libraries.
/src/common/ for code reusable between node.js services and client ui. It may be useful to use it to store business-logic related models and DTOs.
/src/service/ App Server service sources directory. These files are loaded by App Server and available as web-services endpoints at /service/ServiceName
/src/client/ client application sources directory
/src/client/constants/Identifiers.ts Identifiers for IoC container
/src/client/config/inversify.config.ts Inversify container configuration script
/src/client/services/ application services interfaces and implementations. 
/src/client/components/ custom Polymer components directory 



Development notes


Initializing variables in Polymer web components

The main inconvenient side-effect of using Typescript classes as Polymer 1.x web components is losing the ability to use class constructor and initializing class variables at their declaration.
This is probably due to the current Polymer-TS implementation and it might be changed in future or with migration to Polymer 2.
Instead of class constructor and default initizialition the ready() method should be used for initializing the default variable values, or, in case of properties, they should be specified in @property decorator, e.g. @property({type: String, value: "default"})


Injecting dependencies into Polymer web components

Due to the previous limitation, injection via decorators is, unfortunately, impossible into Polymer web components. ready() method is used instead of class constructor and properties for the components, so the workaround  is to get dependencies  from container directly which is otherwise is strongly discouraged.

this.service = container.get<IService>(Identifiers.Service);
This should be limited only to ready() method, preferably in the beginning of it, avoiding service-locator anti-pattern as much as possible.


Further development

Please feel free to participate in the development of this project by opening issues, creating merge requests or forking the project.


TODOs:


  Further Eikon APIs description in Typescript. Possibly creating separate repository for API typings.
  Better testing



Disclaimer

This project was developed for internal needs of TR Russia CIS GGO Technology Team and is provided "AS IS". 
This project may be used as a backbone for other similar projects in Thomson Reuters.
