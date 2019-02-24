# Ext JS Client
If you are not familiar with Ext JS, we strongly recommend to learn the [Ext JS Modern Toolkit Quick Start](http://docs.sencha.com/extjs/latest/guides/quick_start/introduction.html) as a starting point.

## Architecture
In this example, the global view logic is implemented by the [`App.view.viewport.ViewportController`](./app/view/viewport/ViewportController.js) (attached to the application [viewport](http://docs.sencha.com/extjs/latest/modern/Ext.Viewport.html)), which is responsible of:

- the initialization of the [communication protocol](#communication) (Ext.Direct)
- the user session management and authentication (login and logout) 
- the creation of the root views (`login` and `main`)

### Routing
Routes are handled by the [`App.view.viewport.ViewportController`](./app/view/viewport/ViewportController.js) for unauthenticated pages (e.g. `#login`) and by the [`App.view.main.MainController`](./app/view/main/MainController.js) once a user has been successfully authenticated (e.g. `#home`, `#person/{uuid}`, etc.). Read more about [controlling an application with router](http://docs.sencha.com/extjs/latest/guides/application_architecture/router.html).

### Theming
The custom style of the application lives under the following places:
- `packages/local/coworkee` for the style that can be shared between multiple applications (e.g. Ext JS components look and feel)
- `app/` for the style specific for this app (e.g. the login page design), in `.scss` files next to their associate `.js` classes

Learn more about theming by reading [Theming Ext JS](http://docs.sencha.com/extjs/latest/guides/core_concepts/theming.html).

### Communication
This example relies on [Ext.Direct](https://docs.sencha.com/extjs/latest/guides/backend_connectors/direct/specification.html) to communicate with the Node.js server. The configuration of this protocol is done in the [`App.model.Base`](./app/model/Base.js) `schema.proxy` and initialized in the [`App.view.viewport.ViewportController`](./app/view/viewport/ViewportController.js). The Ext.Direct API is defined in the [`server/api/*.js`](../server/api) files, exposed at the `http://localhost:3000/api` endpoint and accessible in the client thought the `Server` global variable (e.g. `Server.auth.login()`).

Authentication is based on [JSON Web Token](https://jwt.io/introduction/#how-do-json-web-tokens-work-) sent along each Ext.Direct request, as part of the `Authorization` header (see `setDirectToken` in [`App.view.viewport.ViewportController`](./app/view/viewport/ViewportController.js)).

## Structure
The root folder contains the top-level pieces of the application:

 - `app.json` - The application descriptor which controls how the application is built and loaded.
 - `app.js` - The file that launches the application, [read more...](http://docs.sencha.com/extjs/latest/guides/application_architecture/application_architecture.html#application_architecture-_-application_architecture_-_app_js)
 - `build.xml` - The entry point for Sencha Cmd to access the generated build script, [read more...](http://docs.sencha.com/cmd/guides/advanced_cmd/cmd_build.html)
 - `bootstrap.*` - These files are generated by Sencha Cmd to enable the application to load in "development mode", [read more...](http://docs.sencha.com/cmd/guides/microloader.html)
 - `index.html` - The default web page for this application (can be customized in `"app.json"`).
 
A detailed description of the Ext JS file structure can be found in [this guide](http://docs.sencha.com/extjs/latest/guides/application_architecture/application_architecture.html#application_architecture-_-application_architecture_-_file_structure).

### `./app`
The `app` folder contains the sources (JavaScript and SASS files) of the application:

 - `app/controller/` - Global / application-level controllers, [read more...](http://docs.sencha.com/extjs/latest/guides/application_architecture/application_architecture.html#application_architecture-_-application_architecture_-_controllers)
 - `app/model/` - Data model classes, [read more...](http://docs.sencha.com/extjs/latest/guides/application_architecture/application_architecture.html#application_architecture-_-application_architecture_-_models)
 - `app/profile/` - App profiles, [read more...](http://docs.sencha.com/extjs/latest/guides/application_architecture/developing_for_multiple_screens_and_environments.html#application_architecture-_-developing_for_multiple_screens_and_environments_-_app_profiles)
 - `app/store/` - Data stores, [read more...](http://docs.sencha.com/extjs/latest/guides/application_architecture/application_architecture.html#application_architecture-_-application_architecture_-_stores)
 - `app/view/` - Views as well as ViewModels and ViewControllers, [read more...](http://docs.sencha.com/extjs/latest/guides/application_architecture/application_architecture.html#application_architecture-_-application_architecture_-_views)

### `./overrides`
This folder contains JavaScript code that alter or extend the behavior of Ext JS  classes. Note that the contents of "overrides" folders are automatically required and included in builds. These should not be explicitly mentioned in "requires" or "uses" in code. [Read more...](http://docs.sencha.com/extjs/latest/guides/other_resources/extjs_faq.html#other_resources-_-extjs_faq_-_how_should_i_override_a_method_without_editing_the_source_code_)

### `./resources`
This folder contains assets such as images, font, etc. [Read more...](http://docs.sencha.com/cmd/guides/resource_management.html#resource_management_-_resource_management)

### `./packages`
This folder contains one [local package](http://docs.sencha.com/cmd/guides/cmd_packages/cmd_packages.html#cmd_packages-_-cmd_packages_-_local_packages) (`packages/local/coworkee`) which defines the application theme. Read more about [Sencha Packages](http://docs.sencha.com/cmd/guides/cmd_packages/cmd_packages.html).

### `./build`
This folder contains the output of the build, inclucing generated CSS files, consolidated resources and concatenated JavaScript files.