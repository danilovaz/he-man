# ![He-Man](https://raw.githubusercontent.com/chrisenytc/he-man/master/logo.png)

> A superhero socket.io framework

[![Build Status](https://secure.travis-ci.org/chrisenytc/he-man.png?branch=master)](http://travis-ci.org/chrisenytc/he-man) [![GH version](https://badge-me.herokuapp.com/api/gh/chrisenytc/he-man.png)](http://badges.enytc.com/for/gh/chrisenytc/he-man)

## Getting Started

1º Clone He-Man repo

```bash
git clone https://github.com/chrisenytc/he-man.git
```

2º Enter in he-man directory
```bash
cd he-man
```

3º Install dependencies

```bash
npm install
```

4º Configure the settings in `api/config`

6º Start He-Man

```bash
npm start
```

Test your He-Man app

```bash
npm test
```

## Documentation

### Socket Controllers

How to use Sockets

The He-Man uses socket.io, you need to follow some conventions to able to use it.

1. The file name and the method name will be used as socket path. e.g: `test.js` + `index` = `test/index`
2. You can listen or emit a message using that path. e.g: `on: function(data){}` or `emit: 'message-example'`
3. `this` variable has the scope of socket.io and can use all of its methods. e.g: `this.on('test/index', function(data){});`, `this.emit('test/index', 'message-example')` and more.

Example:

```javascript

module.exports = function(app, config) {

    function IndexController() {
        //inherits
    }

    IndexController.prototype.index = {
        on: function() {
            return this.emit('index/list', {config: config, service: app.getService('utilsService')});
        },
        emit: 'Hello Frontend!'
    };

    return IndexController;
};

```

#### App Helpers

- app.getConfig(fileName)
- app.getService(fileName)
- app.getModel(fileName)
- app.getLib(fileName)

## Services

How to use services

Services are an abstraction layer that allows you to do all the heavy lifting here and let sockets clean.

Conventions:

- The name of the js file will be used as the name of the service. e.g: `app.getService('utilsService')`

Example:

```javascript
module.exports = function utilsService() {
    return 'Hello World!';
};
```

```javascript
exports.utilsService = function utilsService() {
    return 'Hello World!';
};
```

## Models

How to use Models

The models in He-Man.js uses the mongoose and follows the implementation of the example below:

```javascript
var mongoose = require('mongoose');
var Schema = mongoose.Schema;

/**
 * Taks Schema
 */
var TaskSchema = new Schema({
  created: {
    type: Date,
    default: Date.now
  },
  title: {
    type: String,
    default: '',
    trim: true,
    required: true
  },
  slug: {
    type: String,
    trim: true,
    required: true,
    unique: true
  },
  content: {
    type: String,
    default: '',
    trim: true
  },
  closed: {
    type: Boolean,
    default: false
  }
});

//Exports model
module.exports = mongoose.model('Task', TaskSchema);
```

### Extensions

If you want, you cant to extends the He-Man Core without change any files in the core.

You need to create extensions

How to create socket extensions

Example: 

`lib/extensions/socket.js`

```javascript

module.exports = {
    core: {
        extends: function(io) {
            //Socket Middleware
            io.use(function(req, next) {
                return next();
            });
        }
    }
};
```

## Settings

How to use Settings

The He-Man works with environments, you can have multiple configurations in your application.

The defaults environments are: `development`, `test` and `production`. You also create your own customized reports environments.

You can access the contents of environments using `app.getConfig('nameofconfigfile')`

Conventions:

- The name of the configuration files in `api/config/<env>` are the names used to get the contents of the settings in: `app.getConfig('nameofconfigfile')`


##### Custom Environments

How to create custom environments

1º Create `mycustomenv` folder in `api/config/`

2º Create config files in `api/config/mycustomenv`

3º Run your environment

```bash
NODE_ENV=mycustomenv node app
```

Example:

```bash
NODE_ENV=production node app
```

## Contributing

Please submit all issues and pull requests to the [chrisenytc/he-man](http://github.com/chrisenytc/he-man) repository!

## Support
If you have any problem or suggestion please open an issue [here](https://github.com/chrisenytc/he-man/issues).

## License 

The MIT License

Copyright (c) 2014, Christopher EnyTC

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

