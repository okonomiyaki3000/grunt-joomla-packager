# grunt-joomla-packager

> Tasks for copying extension files from a Joomla installation and repackaging them as an installable package.

## Getting Started
This plugin requires Grunt `~0.4.5`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-joomla-packager --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-joomla-packager');
```

## The "joomla_packager" task

### Overview
In your project's Gruntfile, add a section named `joomla_packager` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  joomla_packager: {
    options: {
      // Task-specific options go here.
    },
    your_target: {
        options: {
            // Target-specific file lists and/or options go here.
        }
    }
  }
});
```

### Options

#### options.name
**Required**
Type: `String`
Default value: `null`

The base name of the extension to be packaged. Omit any obligatory prefix. For example, to package a component called `com_mycompoment`, just use `mycomponent`.  

#### options.type
**Required**
Type: `String`
Default value: `null`

The extension type. Should be one of: `component`, `files`, `language`, `library`, `module`, `plugin`, or `template`. The `package` type extension is not currently supported.

#### options.group
**Required if type is `plugin`**
Type: `String`
Default value: `null`

The plugin group. Joomla's built-in groups are `authentication`, `captcha`, `content`, `editors`, `editors-xtd`, `extension`, `finder`, `quickicon`, `search`, `system`, `twofactorauth`, and `user` but this list is also extensible.

#### options.client
Type: `String`
Default value: `'site'`

The client application in which the extension is installed. Can be either `site` or `administrator`. Only applies to the `language`, `module`, and `template` types.

#### options.packageName
Type: `String`
Default value: `null`

The name of a directory that will be created (inside of `options.dest`) for this package. There's really no need to set this as the value will either be taken from the `name` value in the manifest file or generated based on the type, group (plugins only), and name options.

#### options.packagePrefix
type: `Object`
default value:
```js
{
    'component': 'com',
    'plugin':    'plg',
    'files':     'files',
    'library':   'lib',
    'package':   'pkg',
    'module':    'mod',
    'template':  'tpl',
    'language':  'lan'
}
```

This is a mapping of extension type to naming prefix. This prefix will only be used in cases when the name of the package directory needs to be generated. This should be a rare case.

#### options.joomla
Type: `String`
Default value: `'.'`

The path to your Joomla installation's root directory. Files will be copied from locations inside this directory (in most cases).

#### options.dest
Type: `String`
Default value: `'./dest'`

The location where files should be copied to. A directory will be created in this location for each and every extension being packed. If you are packaging a component called `mycomponent`, a folder `com_mycomponent` will be created here and files will be copied into it.

#### options.administrator
Type: `String`
Default value: `options.joomla + '/administrator'`

The location of the Joomla _administrator_ directory. No need to change this unless using a custom `defines.php` file.

#### options.libraries
Type: `String`
Default value: `options.joomla + '/libraries'`

The location of the _libraries_ directory. No need to change this unless using a custom `defines.php` file.

#### options.plugins
Type: `String`
Default value: `options.joomla + '/plugins'`

The location of the _plugins_ directory. No need to change this unless using a custom `defines.php` file.

#### options.templates
Type: `String`
Default value: `options.joomla + '/templates'`

The location of the _templates_ directory. No need to change this unless using a custom `defines.php` file.

#### options.manifests
Type: `String`
Default value: `options.administrator + '/manifests'`

The location of the _manifests_ directory. No need to change this unless using a custom `defines.php` file.

#### options.adminTemplates
Type: `String`
Default value: `options.administrator + '/templates'`

The location of the administrator _templates_ directory. No need to change this unless using a custom `defines.php` file in the administrator app.

### Usage Examples

#### Component Packaging
In this example, `com_content` is being copied from a Joomla directory and packaged.

```js
grunt.initConfig({
  joomla_packager: {
    com_content: {
        options: {
          'name': 'content',
          'type': 'component',
          'joomla': '/path/to/joomla',
          'dest': '/path/to/packages'            
        }
    }
  }
});
```

> __Fun Fact:__ This configuration actually fails because the manifest file for this component has a mistake in it. You can make it work by using `--force`.

#### Global Options
It's highly likely that you'll be copying from the same joomla directory and to the same destination for more than one package. Then you can do this:

```js
grunt.initConfig({
  joomla_packager: {
    options: {
      'joomla': '/path/to/joomla',
      'dest': '/path/to/packages'
    },
    com_content: {
        options: {
          'name': 'content',
          'type': 'component'
        }
    },
    com_categories: {
        options: {
          'name': 'categories',
          'type': 'component'
        }
    }
  }
});
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History
* 2015-10-05   v0.1.0b10    Minor changes to readme and package.json.
* 2015-10-05   v0.1.0b9   Accept any tag name inside of `<files>` and `<media>`.
* 2015-10-01   v0.1.0b8   Better package naming. Properly locate libraries based on the library name in the manifest file.
* 2015-10-01   v0.1.0b7   Bugfix: various fixes for the 'files' extension type.
* 2015-10-01   v0.1.0b6   Bugfix: create directories for each package.
* 2015-10-01   v0.1.0b5   Bugfix: more logging problems.
* 2015-10-01   v0.1.0b4   Bugfix: logging problems.
* 2015-09-30   v0.1.0b3   Add a 'client' option (instead of repurposing 'group').
* 2015-09-29   v0.1.0b2   Bugfix: default manifest path was wrong.
* 2015-09-28   v0.1.0b1   First beta. Probably works...
