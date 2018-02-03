# express-hateoas-links
[![Shippable branch](https://img.shields.io/shippable/5818b23aa29a9a0f00ba1a28/master.svg)](https://app.shippable.com/projects/5818b23aa29a9a0f00ba1a28) [![npm](https://img.shields.io/npm/dt/express-hateoas-links.svg)](https://www.npmjs.com/package/express-hateoas-links) [![Linked In](https://img.shields.io/badge/Linked-In-blue.svg)](https://www.linkedin.com/in/john-i-doherty) [![Twitter Follow](https://img.shields.io/twitter/follow/MrJohnDoherty.svg?style=social&label=Twitter&style=plastic)](https://twitter.com/MrJohnDoherty)

Extends express res.json to accept an array of HATEOAS links to be appended to the output JSON object.

## Installation

```bash
$ npm install --save express-hateoas-links
```

## Quick usage example

```js
// send person object with HATEOAS links added
res.json(personObject, [
    { rel: "self", method: "GET", href: 'http://127.0.0.1' },
    { rel: "create", method: "POST", title: 'Create Person', href: 'http://127.0.0.1/person' }
]);
```

## Typical use case

The example below adds a self & create link to a JSON schema used to create a person. This allows the consuming application to understand what properties are required to create a Person and the destination URL to post to, removing the need for the application to hard code API links. 

```js
var express = require('express'),
    app = express(),
    hateoasLinker = require('express-hateoas-links');

// replace standard express res.json with the new version
app.use(hateoasLinker);

// standard express route
app.get('/', function(req, res){

    // create an example JSON Schema
    var personSchema = {
        "name": "Person",
        "description": "This JSON Schema defines the paramaters required to create a Person object",
        "properties": {
            "name": {
                "title": "Name",
                "description": "Please enter your full name",
                "type": "string",
                "maxLength": 30,
                "minLength": 1,
                "required": true
            },
            "jobTitle": {
                "title": "Job Title",
                "type": "string"
            },
            "telephone": {
                "title": "Telephone Number",
                "description": "Please enter telephone number including country code",
                "type": "string",
                "required": true
            }
        }
    };

    // call res.json as normal but pass second param as array of links
    res.json(personSchema, [
        { rel: "self", method: "GET", href: 'http://127.0.0.1' },
        { rel: "create", method: "POST", title: 'Create Person', href: 'http://127.0.0.1/person' }
    ]);
});

// express route to process the person creation
app.post('/person', function(req, res){
    // do some stuff with the person data
});

```

## Output

```json
{
    "name": "Person",
    "description": "This JSON Schema defines the paramaters required to create a Person object",
    "properties": {
        "name": {
            "title": "Name",
            "description": "Please enter your full name",
            "type": "string",
            "maxLength": 30,
            "minLength": 1,
            "required": true
        },
        "jobTitle": {
            "title": "Job Title",
            "type": "string"
        },
        "telephone": {
            "title": "Telephone Number",
            "description": "Please enter telephone number including country code",
            "type": "string",
            "required": true
        }
    },
    "links":[
        { 
            "rel": "self", 
            "method": "GET", 
            "href": "http://127.0.0.1"
        },
        { 
            "rel": "create", 
            "method": "POST", 
            "title": "Create Person", 
            "href": "http://127.0.0.1/person"
        }
    ]
}
```

## Tests

You can run the unit tests by changing directory into the express-hateoas-links director within your node_modules folder, and run the following commands:

```bash
npm install   // install modules dev dependencies
npm test      // run unit tests
```

## License

[ISC License](LICENSE) &copy; 2016 [John Doherty](https://twitter.com/CambridgeMVP)