# grunt-loopback-angular-addmodeldata-fork

> This is a fork of Jordan Thomas's grunt-loopback-angular-addmodeldata which extends grunt-loopback-angular. Auto-generating Angular $resource services for loopback will include model data for use at runtime on angular client.

## About the fork

This fork was created and published as a means of properly addressing a bug in grunt-loopback-angular-addmodeldata which will fail to find the correct model.json file in the event of multi-word model name where the default naming convention of the slc model generator is kept. Example: The JSON model config file would not be found if using *slc loopback:model* to generate your model.json files and your model name contained multiple words, such as CamelCaseModelName. The bug in the npm module prior to this fork would attempt to look for CamelCaseModelName.json, when it should be looking for camel-case-model-name.json. Lowercasing of JSON file names has also been added in the event there are issues with your operating system having strict standards on file or directory name casing.

Most of the original author's documentation has been kept below with changes being made where needed to address installation differences.

## Getting Started
This plugin requires Grunt `~0.4.5`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-loopback-angular-addmodeldata-fork --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-loopback-angular-addmodeldata-fork');
```

## The "loopback_angular_addModelData_fork" task

### Overview
In your project's Gruntfile, add a section named `loopback_angular_addModelData_fork` to the data object passed into `grunt.initConfig()`.

Service.options shown are defaults so you can leave them off if using the same. Should look like

```js
loopback_angular_addModelData_fork: {
      services: {
        options: {
          modelConfig: 'server/model-config.json',
          serviceFile: 'client/app/scripts/lb-services.js',
          modelDir: 'common/models/'
        }
      }
    }
```
Register task as part of loopback command

```js
grunt.registerTask('loopback', [
    'loopback_angular',
	  'loopback_angular_addModelData_fork', <-- Add this line
    'docular'
  ]);
```

### Usage Examples
So what do you get for this? Now when you access loopback resources in angular you can get at the model info stored in the model.json file. As in
```js
//Supposing you have exposed a customer model on your loopback api
function(Customer) {
	//figure our what properties the customer model has at runtime
	modelProps = Customer.model.properties;
}
```

Original author (Jordan Thomas) built this because of using [angular-formly](https://github.com/nimbly/angular-formly) to automatically generate forms for editing models. By getting access to model properties at runtime and can build out the forms simply based on the fields defined in model.json
