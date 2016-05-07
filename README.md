[![Build Status](https://travis-ci.org/vandium-io/lambda-tester.svg?branch=master)](https://travis-ci.org/vandium-io/lambda-tester)

# lambda-tester

Simplifies writing unit tests for [AWS Lambda](https://aws.amazon.com/lambda/details) functions using [Node.js](https://nodejs.org).

## Features
* Verifies correct handler behavior
* Works asynchronously like Lambda does
* Detects resource leaks [experimental]
* Supports Promises
* Easily integrates with test frameworks
* Lightweight and won't impact performance
* Maps the environment variable `LAMBDA_TASK_ROOT` to the application's root
* Automatically loads .env files
* Works with Node 4.3.2+

## Installation
Install via npm.

	npm install lambda-tester --save-dev


## Getting Started

Lambda handlers with support for callbacks use the typical Node.js asynchronous signature:

```js
exports.handler = function( event, context, callback ) {

    callback( null, 'success!' );
}
```


The following example shows a simple case for validating that the Lambda (handler) was called successfully (i.e. `callback( null, result )`:

```js
const LambdaTester = require( 'lambda-tester' );

const myHandler = require( '../index' ).handler;

describe( 'handler', function() {

	it( 'test success', function() {

		return LambdaTester( myHandler )
			.event( { name: 'Fred' } )
			.expectResult();
	});
});
```

If the handler decides to call `callback( err )` then the verification will fail and the test will fail.

Additionally, if one wanted to test for failure, then the following code would be used:

```js
const LambdaTester = require( 'lambda-tester' );

const myHandler = require( '../index' ).handler;

describe( 'handler', function() {

	it( 'test failure', function() {

		return LambdaTester( myHandler )
			.event( { name: 'Unknown' } )
			.expectError();
	});
});
```

Please note that you must return the `LambdaTester` back to the framework since `lambda-tester` is asynchronous and uses Promises.

## documentation

Complete documentation can be found in our [documentation](docs/main.md) page.

## Feedback

We'd love to get feedback on how you're using lambda-tester and things we could add to make this tool better. Feel free to contact us at `feedback@vandium.io`

## Compatibility

Version 2.x targets Lambda handlers using Node 4.3.2. If you require support for Node 0.10.36 then use version 1.0.x.


## License

[BSD-3-Clause](https://en.wikipedia.org/wiki/BSD_licenses)
