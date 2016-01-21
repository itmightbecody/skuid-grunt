#skuid-grunt

**skuid-grunt** is a toolkit that enables developers working with Skuid extend their development processes to their Skuid pages.

##Purpose
At Skuid, we eat our own dogfood. The Skuid interface is built using the very software we produce. **skuid-grunt** was born to help our developers easily version and release Skuid pages. Not only does this support our development process, but it also affords you, our customer, the unique opportunity to bring your Skuid pages into the same source control as the rest of your Salesforce code.

##Features
* Pull Skuid pages from a specified org into your local filesystem
* Push Skuid pages from a local filesystem to any Salesforce org running Skuid
* Generate a Skuid Page Pack that can be shared across orgs.

##Requirements
* [Grunt](http://gruntjs.com/)
* [Node.js](https://nodejs.org/)

##Getting Started
Before installing **skuid-grunt**, make sure that you have the above requirements installed.

Install **skuid-grunt**
```bash
$ npm install skuid-grunt
```

##Example Gruntfile
```js
module.exports = function(grunt){
  grunt.initConfig({
    'skuid-pull':{
      'dev':{
        options:{
          'dest': 'src/skuidpages/',
          'clientId': 'demoCliId',
          'clientSecret': 'demoCliSec',
          'username': 'demoUsrnm',
          'password': 'demoPasswd',
          'module':['Module1'], //can be array or CSV
        }
      }  
    },
    'skuid-push':{
      'production':{
         src: ['src/skuidpages/Module1*'],
         options:{
          'clientId': 'demoCliId',
          'clientSecret': 'demoCliSec',
          'username': 'demoUsrnm',
          'password': 'demoPasswd',
        }
      }
    }
  });

  //task that will pull your Skuid pages from a developer org and push them
  //right into a production org
  grunt.registerTask('to-production', ['skuid-pull:dev', 'skuid-push:production']);

  grunt.loadNpmTasks('skuid-grunt');
}
```

##Task Configuration

###skuid-pull
Pull Skuid pages from any Salesforce org with Skuid installed. This task will create 2 files per Skuid page for each module you specify. These files will be named ```ModuleName_PageName.json``` & ```ModuleName_PageName.xml```. The XML file is your Skuid page. You can copy that file and paste it directly into the Skuid XML Editor. The JSON file is additional metadata about your Skuid page that will be used in the **skuid-push** task.
* ```options.dest```: [String|Optional] The target directory where your page files will be written
* ```options.clientId```: [String|Required] The OAuth Client Id of the org you wish to connect
* ```options.clientSecret```: [String|Required] The OAuth Client Secret of the org you wish to connect
* ```options.username```: [String|Required] The username of the org you wish to connect
* ```options.password```: [String|Required] The password of the org you wish to connect 
* ```options.module```: [String or Array| Required] The Module(s) you want to pull down.
* ```options.nforceOptions```: [Object|Optional] Any additional [nforce](https://github.com/kevinohara80/nforce) options you wish to use

###skuid-push
Push Skuid pages from your local directory whether you just pulled them down or have checked out your code from source control. This task will take the page definitions that you specify in ```src``` and push them to your Salesforce org. Once the task is finished, you can log into you org and begin working directly on those pages!
* ```src```: [String or Array|Required] Path to directory that stores your Skuid page definitions, [examples here](http://gruntjs.com/configuring-tasks#files)
* ```options.clientId```: [String|Required] The OAuth Client Id of the org you wish to connect
* ```options.clientSecret```: [String|Required] The OAuth Client Secret of the org you wish to connect
* ```options.username```: [String|Required] The username of the org you wish to connect
* ```options.password```: [String|Required] The password of the org you wish to connect 
* ```options.nforceOptions```: [Object|Optional] Any additional [nforce](https://github.com/kevinohara80/nforce) options you wish to use

###skuid-page-pack
