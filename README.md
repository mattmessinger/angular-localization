# angular-localization

*AngularJS Localization done right. Kudos to the original author, Rahul Doshi, for this fantastic plugin.*
*Due to a slowdown of activities on the main repo, we have forked it since we use it extensively in our angular projects.*
*Feel free to submit PRs!*

[![Build Status](https://travis-ci.org/Spiria-Digital/angular-localization.svg?branch=master)](https://travis-ci.org/Spiria-Digital/angular-localization.svg?branch=master)
[![Code Climate](https://codeclimate.com/github/Spiria-Digital/angular-localization/badges/gpa.svg)](https://codeclimate.com/github/Spiria-Digital/angular-localization)
[![Test Coverage](https://codeclimate.com/github/Spiria-Digital/angular-localization/badges/coverage.svg)](https://codeclimate.com/github/Spiria-Digital/angular-localization/coverage)
[![Bower version](https://badge.fury.io/bo/angular-localization-mattmessinger.svg)](http://badge.fury.io/bo/angular-localization)
[![GitHub license](https://img.shields.io/github/license/Spiria-Digital/angular-localization-mattmessinger.svg)](https://github.com/Spiria-Digital/angular-localization/blob/master/LICENSE)

___

## Table of Contents

- [Overview](#overview)
    - [Build Dependencies](#build-dependencies)
    - [Dear Developer](#dear-developer)
- [Getting Started](#getting-started)
    - [Quick Start](#quick-start)
    - [Wiring It Up](#wiring-it-up)
    - [Module Setup](#module-setup)
    - [Localization File Formats](#localization-file-formats)
    - [Nested Json](#nested-json)
- [Usage Examples](#usage-examples)
    - [i18n directive](#i18n-directive)
        - [Localize Using the i18n attribute](#localize-using-the-i18n-attribute)
        - [Localize with Dynamic User Data](#localize-with-dynamic-user-data)
    - [i18nAttr directive](#i18nattr-directive)
    - [locale service](#locale-service)
    - [i18n filter](#i18n-filter)
    - [Sample App](#sample-app)
- [License](#license)

## Overview

A localization module for [AngularJS](http://angularjs.org/) complete with core service and accompanying filter, directives etc.

It was first made by [Rahul Doshi](https://github.com/doshprompt). Due to the lack of recent activity in the main repo, we have forked this version to better maintain it.

It is based on a number of angularjs localization modules already available out there on the web,
and borrows heavily from the following list including but not limited to:

- http://dailyjs.com/2014/04/08/angular-localize/
- https://github.com/blueimp/angular-localize
- http://alicoding.com/how-to-localized-angularjs-app/

It was inspired by [Jim Lavin's AngularJS Resource Localization Service](https://github.com/lavinjj/angularjs-localizationservice)
who made an excellent first tutorial for his original version at [Coding Smackdown TV](http://codingsmackdown.tv/blog/2012/12/14/localizing-your-angularjs-app/)
which was later updated to include performance improvements [seen here](http://codingsmackdown.tv/blog/2013/04/23/localizing-your-angularjs-apps-update/).

##### Main Points of Difference

1. Simplified format of the translation file.
2. Support for parameterized messages.

##### Major Improvements

1. Parameters in the directive may be bound to `$scope` variables from the nearest parent controller.
2. HTML element tag attributes can also be natively localized.
3. Ability to configure a whole slew of things for more customizability to play nicely with your application.
4. You can used nested json in your language files.

### Build Dependencies

- node.js >= v0.10.x
- npm
- gulp `npm install -g gulp`
- bower `npm install -g bower`

### Dear Developer

##### tl;dr

```shell
$ npm install -d
$ npm test
```

##### Contributing

In lieu of a formal style guide, please take good care to maintain the existing coding style.
Add unit tests for any new or changed functionality. Lint and test your code using [gulp.js](http://gulpjs.com/).
Please refer to this [document][commit-message-format] for a detailed explanation of git commit guidelines - source: [AngularJS](https://angualrjs.org)
[commit-message-format]: https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#

## Getting Started

### Quick Start

The easiest way to install the `ngLocalize` module is via [Bower](http://bower.io/):

```shell
bower install angular-localization-mattmessinger --save
```

Two other options are available:

- [Download the latest release](https://github.com/Spiria-Digital/angular-localization/archive/master.zip).
- Clone the repo: `git clone https://github.com/Spiria-Digital/angular-localization.git`.

You can then include `angular-localization-mattmessinger` after its dependencies,
[angular](https://github.com/angular/bower-angular) and
[angular-cookies](https://github.com/angular/bower-angular-cookies):

```html
<script src="bower_components/angular/angular.js"></script>
<script src="bower_components/angular-cookies/angular-cookies.js"></script>
<script src="bower_components/angular-sanitize/angular-sanitize.js"></script>
<script src="bower_components/angular-localization-mattmessinger/angular-localization-mattmessinger.js"></script>
```

### Wiring It Up

1. Include the required libraries
2. Ensure that you inject `ngLocalize` into your app by adding it to the dependency list.

```js
angular.module('myApp', ['ngLocalize']);
```

### Module Setup

All overridable configuration options are part of `localeConf` within the `ngLocalize.Config` module that comes bundled along with this plugin and works alongside `ngLocalize`

###### basePath @ `languages`
> the folder off the root of your web app where the resource files are located (it can also be used as a relative path starting from the folder where your `index.html` file is located).

###### defaultLocale @ `en-US`
> the locale that your app is initialized with for a new user

###### sharedDictionary @ `common`
> commonly occuring words, phrases, strings etc. are stored in this file (it is used to check whether the service itself is ready or not as it is loaded during bootstrap)

###### fileExtension @ `.lang.json`
> the extension for all resource files spanning across all languages

###### persistSelection @ `true`
> whether to save the selected language to cookies for retrieval on later uses of the app

###### cookieName @ `COOKIE_LOCALE_LANG`
> works in conjuntion with `persistSelection` and provides the cookie name for storage

###### observableAttrs @ `\^data-(?!ng-|i18n)\`
> a regular expression which is used to match which custom data attibutes may be watched by the `i18n` directive for 2-way bindings to replace in a tokenized string

###### delimiter @ `::`
> the delimiter to be used when passing params along with the string token to the service, filter etc.

###### validTokens @ `\^([\\w-]+\\/)*([\\w-]+\\.)+([\\w\\s-])+\\w(:.*)?$\`
> a regular expression which is used to match the names of the keys, so that they do not contain invalid characters. If you want to support an extended character set for the key names you need to change this.

###### allowNestedJson @ `false`
> allows you to use nested json in your languages files. This changes how you must set your i18n tokens. Consult the [Nested Json](#nested-json) section for more details.

```js
angular.module('myApp', [
    'ngLocalize',
    'ngLocalize.Config'
]).value('localeConf', {
    basePath: 'languages',
    defaultLocale: 'en-US',
    sharedDictionary: 'common',
    fileExtension: '.lang.json',
    persistSelection: true,
    cookieName: 'COOKIE_LOCALE_LANG',
    observableAttrs: new RegExp('^data-(?!ng-|i18n)'),
    delimiter: '::',
    validTokens: new RegExp('^([\\w-]+\\/)*([\\w-]+\\.)+([\\w\\s-])+\\w(:.*)?$'),
    allowNestedJson: false
});
```

__NOTE:__ There is one caveat; if you want to override at least one particular option,
then you must explicitly set any other options explicitly to their default values (shown above).

##### Availability of Events Fired

Publicly exposed events may be found as part of `localeEvents` situated within the `ngLocalize.Events` module that comes pre-loaded along with this plugin and is leveraged internally by the service.

- `localeEvents.resourceUpdates`: when a new language file is pulled into memory
- `localeEvents.localeChanges`: when the user chooses a different locale

```js
angular.module('myApp', [
    'ngLocalize',
    'ngLocalize.Events'
]).controller('myAppControl', ['$scope', 'localeEvents',
    function ($scope, localeEvents) {
        $scope.$on(localeEvents.resourceUpdates, function (data) {
            // Example data parameter for fr-FR common.lang.json bundle:
            // {
            //     locale: 'fr-FR',
            //     path: 'common',
            //     bundle: {
            //         "yes": "Oui",
            //         "no": "Aucun"
            //     }
            // }
        });
        $scope.$on(localeEvents.localeChanges, function (event, data) {
            console.log('new locale chosen: ' + data);
        });
    }
]);
```

__NOTE:__ All of these are sent by `$rootScope` so that they may be accessible to all other `$scopes` within your app.
Otherwise you could alternatively always choose to inject `$rootScope` and consume (ingest) the aforementioned events.

##### Declaration of New Languages

This plugin relies on the developer(s) to let it know of all languages currently part of the codebase.
These languages must be defined as per the [Language Culture Name specification] (http://msdn.microsoft.com/en-us/library/ee825488(v=cs.20).aspx).
It is broken up into two parts, both of which are located in the `ngLocalize.InstalledLanguages` modules separated out as `localeSupported` and `localeFallbacks`.
The first is responsible for the list of languages currently supported as the name suggests while the latter takes care of fallbacks when a particular language is present but not for the region from which the app is being opened (e.g. en-GB).
As a last resort, if none of these are valid and/or available, it will fallback to the defaultLocale configured as part of the options during initialization.

```js
angular.module('myApp', [
    'ngLocalize',
    'ngLocalize.InstalledLanguages'
])
.value('localeSupported', [
    'en-US',
    'fr-FR',
])
.value('localeFallbacks', {
    'en': 'en-US',
    'fr': 'fr-FR'
});
```

__NOTE:__ Since the plugin does not rely on auto-discovery mechanisms of any kind, and none of the sort is planned for the future,
it is possible to start creating language resource files before they must be fully integrated.
Simply leaving them out of the declaration will suffice meaning it will cause them to be ignored when deciding on which language to show up on the page(s).

### Localization File Formats

Each localization file is pretty simple. It consists of a flat JSON object:

```json
{
    "helloWorld": "Hello World",
    "yes": "Yes",
    "no": "No"
}
```

The key is used to look up the localized string, the value will be returned from the lookup.

##### Parameterized Substitutions of String Text in Tokens

As mentioned earlier, this plugin is able to handle substitutions of tokens based on arguments passed to it along with the token, provided that the token contains some indication of where it is to be placed within that said token. There are a few ways of doing this:

```json
{
    "helloWorld": "Hello %name"
}
```

```json
{
    "helloWorld": "Hello {1}",
}
```

```json
{
    "helloWorld": "Hello {firstName} {lastName}"
}
```

```json
{
    "helloWorld": "Hello %1 %2"
}
```

Please take note of the fact that multiple ordered params may also be given to it.

## Usage Examples

### i18n directive

#### Localize Using the i18n attribute

The localization key can be defined as the value of the `i18n` attribute:

```html
<any i18n="common.helloWorld"></any>
```

If the attribute value is not a valid token, then it will itself be used as the element's content.

__NOTE:__ Localizations defined by the `i18n` attribute cannot contain HTML tags, as the translation result will be assigned as text, not as HTML.
This limitation enables a slightly faster localization, as no sanitization is required.

#### Localize with Dynamic User Data

It is also possible to provide dynamic user data to the translation function.

The `i18n` directive observes all non-directive `data-*` attributes and passes them as a normalized map of key/value pairs to the translation function:

```html
<p data-i18n="common.helloWorld" data-name="{{ user.name }}"></p>
```

Whenever `user.name` is updated, it's indicator in the token `helloWorld` is subsitituted for the new value when the translation function gets called with an object, e.g. `{ name: 'Bob' }` as an argument and the element content is then updated accordingly.

### Nested json

If you set the ```allowNestedJson``` option to ```true``` in the ```localeConf```, there are few things that change.

First of all, to navigate files and folders, you need to use forward slashes to delimit instead of dots.
Dots now traverse potential objects in a json file instead of traversing the file system.

Example:

```html
<p data-i18n="common.helloWorld" data-name="{{ folder1/folder2/myFile.user.name }}"></p>

```

Will fetch the file `myFile` in the folder1/folder2 directories. Inside that file, it expects to find

```json
{
    "user": {
        "name": "Nom d'usager"
    }
}
```

__Legacy support:__ By default, nested json is disallowed, which means that older project should not encounter any issues.
However, if you have a legacy project that you wish to change to nested json, a refactoring __will__ be necessary (mainly replacing the `.` by `/` in the i18n tokens. ie: `common.errors.required` would need to become `common/errors.required`);

__NOTE:__ When constructing its dictionary, it builds an object for each "namespace" (json files). If you have a file structure as such:

```
-languages
  |-en-US
    |-common
      |-errors.lang.json
    |-common.lang.json
  |-en-FR
    |-...
```

it will bundle up the keys from common and the sub keys from the common folder files together. This should not cause any issues unless you have conflicting keys. For example, if common.lang.json had this key:

```json
{
    "errors": {
        "required": "Requis",
        "notFound": "Introuvable"
    }
}
```

then the bundled `common` dictionnary would have conflicting common.errors objects. As a general guideline, avoiding folders and files of the same name at the same level will avoid these issues.

### i18nAttr directive

The i18n Attribute directive is pretty much the same as the i18n directive except that it expects a json-like object structure represented as a set of key-value pair combinations.
The key corresponds to the HTML attribute to be localized, and the value is the localization resource string token which will be passed to the service itself.

If you want to pass dynamic values for the string, those should come after the value for each key; the series of additional parameters is expected to be appendeded to the token, prepended with a separator so that the directive will walk through and replace the numbered place holders with their values.

__NOTE:__ They work the same way as the original `i18n` directive, but instead of updating the element content, they update their associated HTML attribute.

### locale Service

The `locale` service is an equivalent to the `i18n` directive and can be used to generate localized results in situations where the directive cannot be used:

```js
angular.module('myApp', ['ngLocalize'])
    .controller('exampleCtrl', ['$scope', 'locale',
        function ($scope, locale) {
            locale.ready('common').then(function () {
                $scope.sampleText = locale.getString('common.helloWorld', {
                    firstName: 'John',
                    lastName: 'Smith'
                });
            });
        }
    ]);
```

As you can see, The `locale` service expects the localization key as the first argument and an optional {Object|Array|String} with user data as the second argument.

The promise returns the object containing the localization keys & values:

```js
angular.module('myApp', ['ngLocalize'])
    .controller('exampleCtrl', ['$scope', 'locale',
        function ($scope, locale) {
            locale.ready('common').then(function (res) {
                //res --> { "helloWorld" : "Hello World!", ... }
            });
        }
    ]);
```

### i18n filter

The `i18n` filter provides the same functionality as the service.
It can be useful in templates where the localization strings are dynamic, e.g. for error messages:

```html
<any>{{ errorMessage | i18n }}</any>
```

It is also possible to pass an object or an array with localization arguments or even a single string to the `i18n` filter:

```html
<p>{{ errorMessage | i18n:data }}</p>
```

The filter also serves a special purpose targeted at maximum compatibilty with other third party plugin modules, directives, components etc. where it is not possible to provide the parameters as a separate argument.
In such a situation, the token is modified to have the substitution text params appended to it like so:

```js
angular.module('myApp', ['ngLocalize'])
    .controller('exampleCtrl', ['$scope', '$filter', 'locale',
        function ($scope, $filter, locale) {
            locale.ready('common').then(function () {
                $scope.sampleText = $filter('i18n')('common.helloWorld::["John", "Smith"]');
            });
        }
    ]);
```

Notice how we use the `delimiter` from `ngLocalize.Config`'s `localeConf` which can be changed to suit different needs and requirements as well as you see fit in your own app.
The part after the token must be in a valid JSON format as either an array, object or simple string (if it is a single param that needs to be passed in).

### Sample App

I've created a sample app that uses this plugin to provide the text for the entire application.
I registered 'ngLocalize' in my app's dependency list and I then use a combination of
`ng-bind="'home.title' | i18n"`,
`{{ 'home.title' | i18n }}`,
`data-i18n="home.title"` and
`data-i18n-attr="{placeholder: 'home.title'}`
to insert the text into the page at run time along with their variously supported use-cases.

## License

The MIT License (MIT)

Copyright (c) 2014 Rahul Doshi

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

---
