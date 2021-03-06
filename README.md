## Angular-CLI

[![Join the chat at https://gitter.im/angular/angular-cli](https://badges.gitter.im/angular/angular-cli.svg)](https://gitter.im/angular/angular-cli?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![Build Status][travis-badge]][travis-badge-url]
[![Dependency Status][david-badge]][david-badge-url]
[![devDependency Status][david-dev-badge]][david-dev-badge-url]
[![npm][npm-badge]][npm-badge-url]

Prototype of a CLI for Angular 2 applications based on the [ember-cli](http://www.ember-cli.com/) project.

## Note

This project is very much still a work in progress.

The CLI is now in beta. 
If you wish to collaborate while the project is still young, check out [our issue list](https://github.com/angular/angular-cli/issues).

## Prerequisites

The generated project has dependencies that require **Node 4 or greater**.

## Table of Contents

* [Installation](#installation)
* [Usage](#usage)
* [Generating a New Project](#generating-and-serving-an-angular2-project-via-a-development-server)
* [Generating Components, Directives, Pipes and Services](#generating-other-scaffolds)
* [Generating a Route](#generating-a-route)
* [Creating a Build](#creating-a-build)
* [Running Unit Tests](#running-unit-tests)
* [Running End-to-End Tests](#running-end-to-end-tests)
* [Deploying the App via GitHub Pages](#deploying-the-app-via-github-pages)
* [Linting and formatting code](#linting-and-formatting-code)
* [Support for offline applications](#support-for-offline-applications)
* [Commands autocompletion](#commands-autocompletion)
* [CSS preprocessor integration](#css-preprocessor-integration)
* [3rd Party Library Installation](#3rd-party-library-installation)
* [Known Issues](#known-issues)

## Installation

**BEFORE YOU INSTALL:** please read the [prerequisites](#prerequisites)
```bash
npm install -g angular-cli
```

## Usage

```bash
ng --help
```

### Generating and serving an Angular2 project via a development server

```bash
ng new PROJECT_NAME
cd PROJECT_NAME
ng serve
```
Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

You can configure the default HTTP port and the one used by the LiveReload server with two command-line options :

```bash
ng serve --port 4201 --live-reload-port 49153
```

### Generating other scaffolds

You can use the `ng generate` (or just `ng g`) command to generate Angular components:

```bash
ng generate component my-new-component
ng g component my-new-component # using the alias

# components support relative path generation
# if in the directory src/app/feature/ and you run
ng g component new-cmp
# your component will be generated in src/app/feature/new-cmp
# but if you were to run
ng g component ../newer-cmp
# your component will be generated in src/app/newer-cmp
```
You can find all possible blueprints in the table below:

Scaffold  | Usage
---       | ---
Component | `ng g component my-new-component`
Directive | `ng g directive my-new-directive`
Pipe      | `ng g pipe my-new-pipe`
Service   | `ng g service my-new-service`

### Generating a route

You can generate a new route with the following command (note the singular
used in `hero`):

```bash
ng generate route hero
```

This will create a folder which will contain the hero component and related test and style files.

The generated route will also be registered with the parent component's `@RouteConfig` decorator.

By default the route will be designated as a **lazy** route which means that it will be loaded into the browser when needed, not upfront as part of a bundle.

In order to visually distinguish lazy routes from other routes the folder for the route will be prefixed with a `+` per the above example the folder will be named `+hero`.
This is done in accordance with the [style guide](https://angular.io/styleguide#!#prefix-lazy-loaded-folders-with-).

The default lazy nature of routes can be turned off via the lazy flag (`--lazy false`)

There is an optional flag for `skip-router-generation` which will not add the route to the parent component's `@RouteConfig` decorator.

### Creating a build

```bash
ng build
```

The build artifacts will be stored in the `dist/` directory.

### Environments

At build time, the `src/client/app/environment.ts` will be replaced by either
`config/environment.dev.ts` or `config/environment.prod.ts`, depending on the
current cli environment.

Environment defaults to `dev`, but you can generate a production build via
the `-prod` flag in either `ng build -prod` or `ng serve -prod`.

### Running unit tests

```bash
ng test
```

Tests will execute after a build is executed via [Karma](http://karma-runner.github.io/0.13/index.html), and it will automatically watch your files for changes.

You can run tests a single time via `--watch=false`, and turn off building of the app via `--build=false` (useful for running it at the same time as `ng serve`).

**WARNING:** On Windows, `ng test` is hitting a file descriptor limit (see https://github.com/angular/angular-cli/issues/977).
The solution for now is to instead run `ng serve` and `ng test --build=false` in separate console windows. 


### Production Build

When using the `-prod` flag for the `ng build` command, you will generate a single distribution file, which includes all your compiled code and the associated vendors.

The Angular CLI uses the [systemjs-builder](https://github.com/systemjs/builder), to generate the bundle file.

By default the bundler will minify your files and fetch the given source maps.
You are able to overwrite the default `bundler` options, by passing the `bundleOptions` property on the `Angular2App` object.

```js
module.exports = function(defaults) {
  return new Angular2App(defaults, {
    bundleOptions: {
      minify: false,
      sourceMaps: false
    }
  });
}
```

After the production build is ready, the CLI will additionally compress all your files.

### Running end-to-end tests

```bash
ng e2e
```

Before running the tests make sure you are serving the app via `ng serve`.

End-to-end tests are ran via [Protractor](https://angular.github.io/protractor/).


### Deploying the app via GitHub Pages

You can deploy your apps quickly via:

```
ng github-pages:deploy --message "Optional commit message"
```

This will do the following:

- creates GitHub repo for the current project if one doesn't exist
- rebuilds the app in production mode at the current `HEAD`
- creates a local `gh-pages` branch if one doesn't exist
- moves your app to the `gh-pages` branch and creates a commit
- edit the base tag in index.html to support github pages
- pushes the `gh-pages` branch to github
- returns back to the original `HEAD`

Creating the repo requires a token from github, and the remaining functionality
relies on ssh authentication for all git operations that communicate with github.com.
To simplify the authentication, be sure to [setup your ssh keys](https://help.github.com/articles/generating-ssh-keys/).

If you are deploying a [user or organization page](https://help.github.com/articles/user-organization-and-project-pages/), you can instead use the following command:

```
ng github-pages:deploy --user-page --message "Optional commit message"
```

This command pushes the app to the `master` branch on the github repo instead
of pushing to `gh-pages`, since user and organization pages require this.


### Linting and formatting code

You can lint your app code by running `ng lint`.
This will use the `lint` npm script that in generated projects uses `tslint`.

You can modify the these scripts in `package.json` to run whatever tool you prefer.

### Support for offline applications

The index.html file includes a commented-out code snippet for installing the angular2-service-worker. This support is experimental, please see the angular/mobile-toolkit project for documentation on how to make use of this functionality.

### Commands autocompletion

To turn on auto completion use the following commands:

For bash:
```bash
ng completion >> ~/.bashrc
source ~/.bashrc
```

For zsh:
```bash
ng completion >> ~/.zshrc
source ~/.zshrc
```

Windows users using gitbash:
```bash
ng completion >> ~/.bash_profile
source ~/.bash_profile
```


### CSS Preprocessor integration

We support all major CSS preprocessors:
- sass (node-sass)
- less (less)
- compass (compass-importer + node-sass)
- stylus (stylus)

To use one just install for example `npm install node-sass` and rename `.css` files in your project to `.scss` or `.sass`. They will be compiled automatically.

The `Angular2App`'s options argument has `sassCompiler`, `lessCompiler`, `stylusCompiler` and `compassCompiler` options that are passed directly to their respective CSS preprocessors.

### 3rd Party Library Installation

The installation of 3rd party libraries are well described at our [Wiki Page](https://github.com/angular/angular-cli/wiki/3rd-party-libs)

## Known issues

This project is currently a prototype so there are many known issues. Just to mention a few:

- All blueprints/scaffolds are in TypeScript only, in the future blueprints in all dialects officially supported by Angular will be available.
- On Windows you need to run the `build` and `serve` commands with Admin permissions, otherwise the performance is not good.
- The initial installation as well as `ng new` take too long because of lots of npm dependencies.
- Many existing ember addons are not compatible with Angular apps built via angular-cli.
- When you `ng serve` remember that the generated project has dependencies that require **Node 4 or greater**.


## Development Hints for hacking on angular-cli

### Working with master

```bash
git clone https://github.com/angular/angular-cli.git
cd angular-cli
npm link
```

`npm link` is very similar to `npm install -g` except that instead of downloading the package
from the repo, the just cloned `angular-cli/` folder becomes the global package.
Any changes to the files in the `angular-cli/` folder will immediately affect the global `angular-cli` package,
allowing you to quickly test any changes you make to the cli project.

Now you can use `angular-cli` via the command line:

```bash
ng new foo
cd foo
npm link angular-cli
ng serve
```

`npm link angular-cli` is needed because by default the globally installed `angular-cli` just loads
the local `angular-cli` from the project which was fetched remotely from npm.
`npm link angular-cli` symlinks the global `angular-cli` package to the local `angular-cli` package.
Now the `angular-cli` you cloned before is in three places:
The folder you cloned it into, npm's folder where it stores global packages and the `angular-cli` project you just created.

Please read the official [npm-link documentation](https://www.npmjs.org/doc/cli/npm-link.html)
and the [npm-link cheatsheet](http://browsenpm.org/help#linkinganynpmpackagelocally) for more information.


## License

MIT


[travis-badge]: https://travis-ci.org/angular/angular-cli.svg?branch=master
[travis-badge-url]: https://travis-ci.org/angular/angular-cli
[david-badge]: https://david-dm.org/angular/angular-cli.svg
[david-badge-url]: https://david-dm.org/angular/angular-cli
[david-dev-badge]: https://david-dm.org/angular/angular-cli/dev-status.svg
[david-dev-badge-url]: https://david-dm.org/angular/angular-cli#info=devDependencies
[npm-badge]: https://img.shields.io/npm/v/angular-cli.svg
[npm-badge-url]: https://www.npmjs.com/package/angular-cli
