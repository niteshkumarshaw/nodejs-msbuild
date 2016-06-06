# msbuild

msbuild.exe for node.js

Clean, Build, Package, Publish using publish profiles and params.


# example
Build.
``` js
var _msbuild = require('msbuild');
var msbuild = new _msbuild(); 
msbuild.sourcePath = 'c:/your_app.sln';
msbuild.build();
```


# install

With [npm](https://www.npmjs.com/) do:

```
npm install -g msbuild
```


# example 
Visual Studio 2015 example 
``` js
var _msbuild = require('msbuild');
var msbuild = new _msbuild(function(){});
msbuild.sourcePath = 'c:/your_app.sln';
msbuild.configuration='Release';
msbuild.publishProfile='Production_Environment';

var overrideParams = [];
	overrideParams.push('/p:VisualStudioVersion=14.0');
	overrideParams.push('/tv:14.0');
	overrideParams.push('/p:allowUntrustedCertificate=true');
	overrideParams.push('/P:Password=myp@assword');
	
msbuild.config('overrideParams',overrideParams);
msbuild.publish(); 
```
'/p:VisualStudioVersion=14.0' sets version location C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v14.0
'/tv:14.0' sets proj file targets


# example 
Publish w/ verbose logging.
``` js
var _msbuild = require('msbuild');
var msbuild = new _msbuild();
msbuild.sourcePath = 'c:/your_app.sln';
msbuild.configuration = 'your_app_configuration';
msbuild.publishProfile='your_app_publish_profile';
msbuild.verbose = true; 
msbuild.publish();
```


# example 
Preview cmd(s) before running by overriding exec. Can be helpful when troubleshooting.
``` js
var _msbuild = require('msbuild');
var msbuild = new _msbuild();
msbuild.sourcePath = 'c:/your_app.sln';
msbuild.configuration = 'your_app_configuration';
msbuild.publishProfile='your_app_publish_profile';
msbuild.exec  = function(cmd){
	console.log('\nTEST 1: Preview MSBUILD Command');
	console.log('********** test - start ************');
	console.log(cmd);
	console.log('********** test - end  ************\n');
}
msbuild.publish();
```


# example
Adding a callback.
``` js
var your_callback = function(){
	console.log('msbuild done. move on...');
}	
var _msbuild = require('msbuild');
var msbuild = new _msbuild(your_callback); 
msbuild.sourcePath = 'c:/your_app.sln';
msbuild.configuration = 'your_app_configuration';
msbuild.publishProfile='your_app_publish_profile';
msbuild.build();
```

additional configuration parameters
- `os` currently only support windows
- `processor` 	'x86', 'x64'
- `version`	tools version; determines local path to msbuild.exe
- `sourcePath`  'c:/mypath/mysolution.sln'   or   'c:/mypath/myproject.csproj
- `configuration` 	solution configurations; targets an environment (debug,release)  
- `publishProfile`  publish profile; targets a specific machine (app01,app02)
- `outputPath`  package deploy path
- `overrideParams`  property overrides ['/p:WarningLevel=2','/p:OutputDir=bin\Debug','/tv:4.0']  


# notes / updates

### View git project for more examples.

### Linux support added in version: "0.2.1" (thanks to richorama)

### Msbuild version "0.2.4" now available. 
```
Changes include: 
1. overrideParams validation has been corrected to support single character parameters 
  Example: "/m"
2. no longer requires "configuration" 
3. no longer requires "publishProfile"

"sourcePath" is required if the "configuration" and "publishProfile" are not defined
```


# FAQ
How do I resolve error MSB8020?

`C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V120\Microsoft.Cpp.Platform.targets(64,5): error MSB8020: The build to
ols for Visual Studio 2012 (Platform Toolset = 'v110') cannot be found.`

Answer: 
include overrideParams(`--msvs_version=2012`) or update your csproj files
	
